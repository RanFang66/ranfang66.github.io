---
title: Singleton_Design_Pattern_In_Qt
date: 2026-04-22 16:56:53
categories: 
  - Development
tags:
  - Qt
  - C++	
  - Design Pattern
---



## 问题描述

在开发的Qt C++项目中，发现每次程序退出时都会发生崩溃，通过调试发现崩溃发生在单例对象的析构方法中。

### 问题分析

原代码使用了函数局部静态对象的单例模式：                                                                                         

```c++
static ExperimentsBrowser *instance() 
{
    static ExperimentsBrowser instance("Experiments Browser");  // 静态存储                                       return &instance;
}                           
```

 当 MainWindow::initDockWidgets() 调用 addDockWidget() 或 splitDockWidget() 时，Qt 会把 dock widget 重新设置为 MainWindow      的子对象（Qt 文档："If dockwidget is not a child of this main window, it becomes a child."）。                                  

退出流程：                                                                                                                      
    1. main() 返回 → 栈上 scrollArea 析构 → 删除 MainWindow
    2. MainWindow 析构 → Qt 遍历子对象树，delete 了单例 dock widget（第一次析构）                                                   
    3. main() 完全返回后，C++ 运行时按逆构造顺序析构静态存储对象 → 又对已释放的内存调一次析构 → 崩溃

<!--more-->

同样的问题也影响 CytometerController：它持有 QThread，析构器调用 quit()/wait()，如果在 QApplication 销毁后运行，Qt 事件系统已经消亡，threads 无法正常退出，容易挂起或崩溃。 

## Meyer's Singleton 的优势与局限                                                                                                              

###   推荐 Meyer's 的原因                                                                                                         

    1. 线程安全的初始化（C++11 标准保证）                                                                                                                                                                                                                        

```c++
static Foo& instance() 
{                 
    static Foo inst;   // C++11 标准保证多线程下只初始化一次
    return inst;                                                                                             
}
```

 编译器必须生成原子性的"首次初始化"代码（通常用 double-checked locking 或类似机制）。相比之下，堆分配的 static Foo* inst = new   Foo() 虽然在 C++11 下也是线程安全的，但手写的 if (!inst) inst = new Foo 版本（老代码里很多）是有数据竞争的。
                                                                                                                                  

  2. RAII 自动清理

  这是核心优势。Meyer's 单例的析构器一定会执行，意味着：                                                                          
  - 文件缓冲会被 flush
  - 数据库连接会发送 COMMIT/ROLLBACK 并优雅断开                                                                                   
  - 网络 socket 会发送 FIN 关闭                
  - 日志会 flush 到磁盘                                                                                                           
  - 锁、信号量、共享内存会释放                                                                                                    
  - 持有的 OS 资源（管道、句柄等）会归还                                                                                                                                                                                                                        

  堆分配的"泄漏型"单例跳过了所有这些。OS 回收的是内存，不是你的语义资源。例如：                                                   
  - 没 flush 的日志 → 崩溃场景下最后几条消息丢失                                                                                  
  - 没 COMMIT 的数据库事务 → 数据丢失                                                                                             
  - 没关的 socket → 对端长时间保持半开状态                                                                                                                                                                                                                      
    3. 没有动态分配                                                                                                                                                                                                                                              

  对象放在 BSS/data 段，不走堆分配器。启动快，不会因为堆碎片影响其它分配。                                                                                                                                                                                          
    4. Valgrind/ASan 干净                                                                                                                        

  "永不释放"的堆单例在内存检测工具眼里就是泄漏——虽然是所谓的"良性泄漏"，但在代码库充满这种模式时，真正的泄漏就被淹没了。                           
    5. 避免手写 new/delete                                                                                                                       

  现代 C++ 的口号之一是"don't use raw new/delete"，Meyer's 不需要。                                                                                 
---
##   为什么在 Qt 里会炸

### 两套生命周期系统管理导致的崩溃 

根本原因不是 Meyer's 本身不好，而是同一对象被两个独立的生命周期系统管理：

| 系统       | 管理方式                   | 何时析构          |
| ---------- | -------------------------- | ----------------- |
| C++ 运行时 | LIFO 静态析构              | main() 返回之后   |
| Qt 父子树  | 父对象析构时 delete 子对象 | MainWindow 析构时 |

只要对象同时被两边持有（把 QDockWidget 加到 QMainWindow 就会发生），就必然双析构。这是 Meyer's 与外部所有权系统冲突的通用模式，不限于 Qt：                                                                        

  - Python C 扩展中被 Python GC 管理的对象
  - COM 引用计数对象                                                                                                              
  - 由脚本引擎管理的对象
                                                                                                                                  
---
###   静态析构顺序陷阱（Static Destruction Order Fiasco）

  Meyer's 还有另一个陷阱：多个单例互相依赖时，析构顺序是"逆构造顺序"，而不是"依赖顺序"。

```c++
  Logger& Logger::instance() { static Logger l; return l; }
  Database& Database::instance() { static Database d; return d; }   
  
  // 假设 Database::~Database() 里写了 Logger::instance().log("closing");                                       // 如果 Logger 比 Database 先构造，就会比 Database 先析构
  // 那么 Database 析构时访问 Logger 就是 use-after-destroy              
```


  C++ 标准保证只能做到"先构造的后析构"，但无法保证构造顺序匹配你的依赖图。                                                                                                                                                                                          

---
###   各种方案的真实取舍                      

| 方案                 | RAII清理 | 线程安全 | 抗外部所有权 | 抗析构顺序 |
| -------------------- | -------- | -------- | ------------ | ---------- |
| Meyer's 静态         | ✅        | ✅        | ❌            | ？         |
| 堆分配+手动删        | ❌        | ✅        | ✅            | ✅          |
| 堆分配+不删          | ✅        | ✅        | ？           | ？         |
| std::unique_ptr 静态 | ✅        | ✅        | ❌            | ？         |
| Q_GLOBAL_STATIC      | ✅        | ✅        | ？           | ✅          |

