---
layout: page
title: All Posts
subtitle: Browse through all my blog posts
permalink: /posts/
---

<div class="posts-archive">
    {% assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
    
    {% for year in posts_by_year %}
        <div class="year-section">
            <h2 class="year-title">{{ year.name }}</h2>
            
            <div class="posts-list">
                {% for post in year.items %}
                    <article class="post-item">
                        <div class="post-date">
                            {{ post.date | date: "%b %d" }}
                        </div>
                        <div class="post-info">
                            <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
                            <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 20 }}</p>
                            {% if post.tags %}
                            <div class="post-tags">
                                {% for tag in post.tags %}
                                <span class="tag">{{ tag }}</span>
                                {% endfor %}
                            </div>
                            {% endif %}
                        </div>
                    </article>
                {% endfor %}
            </div>
        </div>
    {% endfor %}
    
    {% if site.posts.size == 0 %}
        <div class="no-posts">
            <h3>No posts yet!</h3>
            <p>I'm working on creating some great content. Check back soon!</p>
        </div>
    {% endif %}
</div>

<style>
.posts-archive {
    max-width: 800px;
    margin: 0 auto;
}

.year-section {
    margin-bottom: 3rem;
}

.year-title {
    font-size: 1.5rem;
    font-weight: 600;
    color: var(--primary-color);
    margin-bottom: 1.5rem;
    padding-bottom: 0.5rem;
    border-bottom: 2px solid var(--primary-color);
}

.posts-list {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.post-item {
    display: flex;
    gap: 1.5rem;
    padding: 1.5rem;
    background: var(--background);
    border: 1px solid var(--border-color);
    border-radius: var(--border-radius);
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.post-item:hover {
    transform: translateY(-2px);
    box-shadow: var(--shadow-lg);
}

.post-date {
    flex-shrink: 0;
    width: 80px;
    color: var(--text-light);
    font-weight: 500;
    font-size: 0.9rem;
}

.post-info {
    flex: 1;
}

.post-info h3 {
    margin-bottom: 0.5rem;
}

.post-info h3 a {
    color: var(--text-color);
    text-decoration: none;
}

.post-info h3 a:hover {
    color: var(--primary-color);
}

.post-excerpt {
    color: var(--text-light);
    margin-bottom: 0.5rem;
    line-height: 1.5;
}

.no-posts {
    text-align: center;
    padding: 3rem;
    color: var(--text-light);
}

@media (max-width: 768px) {
    .post-item {
        flex-direction: column;
        gap: 1rem;
    }
    
    .post-date {
        width: auto;
    }
}
</style>