# RanFang's Personal Blog

Welcome to my personal blog hosted on GitHub Pages! This is where I share my thoughts, experiences, and insights on various topics including technology, programming, and personal growth.

## 🌐 Live Site

Visit the blog at: [https://ranfang66.github.io](https://ranfang66.github.io)

## 🛠️ Built With

- **Jekyll** - Static site generator
- **GitHub Pages** - Free hosting
- **Markdown** - Content writing
- **HTML/CSS/JavaScript** - Custom styling and functionality

## 📝 Features

- Responsive design that works on all devices
- Clean, modern layout focused on readability
- Blog post archive and categorization
- SEO optimized with meta tags and sitemap
- RSS feed for content syndication
- About page and contact information

## 🚀 Local Development

To run this blog locally:

1. **Prerequisites**: Install Ruby and Bundler
   ```bash
   gem install bundler
   ```

2. **Clone the repository**:
   ```bash
   git clone https://github.com/RanFang66/ranfang66.github.io.git
   cd ranfang66.github.io
   ```

3. **Install dependencies**:
   ```bash
   bundle install
   ```

4. **Run the development server**:
   ```bash
   bundle exec jekyll serve
   ```

5. **View the site**: Open http://localhost:4000 in your browser

## 📁 Project Structure

```
├── _config.yml          # Jekyll configuration
├── _layouts/            # Page templates
│   ├── default.html     # Base layout
│   ├── home.html        # Homepage layout
│   ├── post.html        # Blog post layout
│   └── page.html        # Static page layout
├── _includes/           # Reusable components
│   ├── header.html      # Site header
│   └── footer.html      # Site footer
├── _posts/              # Blog posts (Markdown)
├── _sass/               # SCSS partials
├── assets/              # CSS, JS, images
│   └── css/
│       └── main.css     # Main stylesheet
├── about.md             # About page
├── posts.md             # Posts archive page
├── index.md             # Homepage
└── Gemfile              # Ruby dependencies
```

## ✍️ Writing Posts

Blog posts are written in Markdown and stored in the `_posts` directory. Each post must:

1. Follow the naming convention: `YYYY-MM-DD-title.md`
2. Include front matter with layout, title, date, and other metadata
3. Be written in Markdown format

Example post structure:
```markdown
---
layout: post
title: "Your Post Title"
date: 2024-01-15 10:00:00 +0000
author: RanFang
tags: [tag1, tag2, tag3]
excerpt: "A brief description of your post"
---

Your post content here...
```

## 🎨 Customization

The blog uses a custom CSS framework with CSS variables for easy theming. Key customization points:

- **Colors**: Edit CSS variables in `assets/css/main.css`
- **Fonts**: Update the Google Fonts import in the default layout
- **Layout**: Modify the layout files in `_layouts/`
- **Content**: Update `_config.yml` for site-wide settings

## 📋 Todo

- [ ] Add search functionality
- [ ] Implement dark mode toggle
- [ ] Add commenting system
- [ ] Create category pages
- [ ] Add social media sharing buttons

## 🤝 Contributing

This is a personal blog, but if you notice any issues or have suggestions:

1. Open an issue to discuss proposed changes
2. Fork the repository
3. Create a feature branch
4. Make your changes
5. Submit a pull request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 📞 Contact

- **GitHub**: [@RanFang66](https://github.com/RanFang66)
- **Email**: ranfang66@example.com
- **Blog**: [https://ranfang66.github.io](https://ranfang66.github.io)

---

*Built with ❤️ using Jekyll and GitHub Pages*