---
layout: post
title: "Getting Started with GitHub Pages: A Simple Guide"
date: 2024-01-20 14:30:00 +0000
author: RanFang
tags: [github-pages, jekyll, tutorial, web-development]
excerpt: "Learn how to create your own personal website or blog using GitHub Pages. This comprehensive guide covers everything from setup to deployment."
---

GitHub Pages is an incredible service that allows you to host static websites directly from your GitHub repositories - and it's completely free! Whether you want to create a personal blog, showcase your portfolio, or document your projects, GitHub Pages makes it easy to get online.

## What is GitHub Pages?

GitHub Pages is a static site hosting service that takes HTML, CSS, and JavaScript files straight from a repository on GitHub, optionally runs the files through a build process, and publishes a website.

### Key Benefits:

- **Free hosting** for personal and open-source projects
- **Custom domain support** with SSL certificates
- **Automatic deployment** from your Git repository
- **Jekyll integration** for easy blogging
- **Version control** for your entire website

## Setting Up Your First GitHub Pages Site

Let's walk through creating a simple personal website.

### Step 1: Create a Repository

1. Go to [GitHub](https://github.com) and create a new repository
2. Name it `yourusername.github.io` (replace with your actual GitHub username)
3. Make sure it's public
4. Initialize with a README

### Step 2: Clone and Setup

```bash
# Clone your repository
git clone https://github.com/yourusername/yourusername.github.io.git
cd yourusername.github.io

# Create a simple index.html
echo "Hello, World!" > index.html

# Commit and push
git add .
git commit -m "Initial commit"
git push origin main
```

### Step 3: Access Your Site

Your site will be available at `https://yourusername.github.io` within a few minutes!

## Using Jekyll for Blogging

Jekyll is a static site generator that's perfect for blogs. GitHub Pages has built-in support for Jekyll, making it incredibly easy to create a blog.

### Basic Jekyll Structure

```
your-site/
├── _config.yml      # Configuration file
├── _posts/          # Blog posts go here
├── _layouts/        # Page templates
├── _includes/       # Reusable components
├── assets/          # CSS, JS, images
└── index.md         # Home page
```

### Creating Your First Post

Posts are written in Markdown and stored in the `_posts` directory. The filename format is important:

```
YEAR-MONTH-DAY-title.md
```

Example: `2024-01-20-my-first-post.md`

```markdown
---
layout: post
title: "My First Post"
date: 2024-01-20 14:30:00 +0000
categories: blog
---

# Welcome to my blog!

This is my first post using Jekyll and GitHub Pages.
```

## Configuration with _config.yml

The `_config.yml` file controls your site's configuration:

```yaml
title: Your Blog Title
description: A brief description of your blog
author: Your Name
email: your.email@example.com
baseurl: ""
url: "https://yourusername.github.io"

# Build settings
markdown: kramdown
theme: minima
plugins:
  - jekyll-feed
  - jekyll-sitemap
```

## Customizing Your Design

You can customize your site in several ways:

### 1. Choose a Theme

GitHub Pages supports several built-in themes:

```yaml
# In _config.yml
theme: minima
# or
remote_theme: pages-themes/minimal@v0.2.0
```

### 2. Custom CSS

Create `assets/css/style.scss`:

```scss
---
---

@import "{{ site.theme }}";

/* Your custom styles here */
.site-header {
  background-color: #2c3e50;
}
```

### 3. Custom Layouts

Create custom layouts in the `_layouts` directory:

```html
<!-- _layouts/post.html -->
<!DOCTYPE html>
<html>
<head>
  <title>{{ page.title }} - {{ site.title }}</title>
</head>
<body>
  <article>
    <h1>{{ page.title }}</h1>
    <time>{{ page.date | date: "%B %d, %Y" }}</time>
    {{ content }}
  </article>
</body>
</html>
```

## Advanced Features

### Custom Domains

1. Add a `CNAME` file to your repository root with your domain
2. Configure your domain's DNS to point to GitHub Pages
3. Enable HTTPS in your repository settings

### Collections

Organize content beyond blog posts:

```yaml
# _config.yml
collections:
  projects:
    output: true
    permalink: /:collection/:name/
```

### Plugins

Extend functionality with Jekyll plugins:

```yaml
# _config.yml
plugins:
  - jekyll-feed        # RSS feed
  - jekyll-sitemap     # XML sitemap
  - jekyll-seo-tag     # SEO meta tags
```

## Best Practices

1. **Use semantic HTML** for better accessibility
2. **Optimize images** to improve loading times
3. **Write descriptive commit messages** for better version control
4. **Test locally** using Jekyll before pushing
5. **Use responsive design** for mobile compatibility

## Local Development

To test your site locally:

```bash
# Install Jekyll (requires Ruby)
gem install bundler jekyll

# Create a Gemfile
echo 'source "https://rubygems.org"' > Gemfile
echo 'gem "github-pages", group: :jekyll_plugins' >> Gemfile

# Install dependencies
bundle install

# Serve locally
bundle exec jekyll serve
```

Your site will be available at `http://localhost:4000`

## Troubleshooting Common Issues

### Build Failures
- Check the Pages settings in your repository
- Review the build logs for error messages
- Ensure your `_config.yml` syntax is correct

### Styling Issues
- Clear your browser cache
- Check that CSS files are properly linked
- Verify your SCSS syntax

### Content Not Updating
- GitHub Pages can take a few minutes to update
- Check that your commits are pushed to the correct branch

## Conclusion

GitHub Pages is a powerful platform for hosting static websites and blogs. With Jekyll integration, you can create sophisticated sites with minimal effort. The combination of version control, free hosting, and automatic deployment makes it an excellent choice for personal projects.

Whether you're showcasing your portfolio, writing a technical blog, or documenting your projects, GitHub Pages provides the tools you need to establish your presence on the web.

## Next Steps

- Explore different Jekyll themes
- Learn about Jekyll plugins
- Consider adding a commenting system
- Set up analytics to track your visitors

Happy blogging! 🚀