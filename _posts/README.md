# How to Add Posts

## Quick Start

To add a new post, create a file in the `_posts` directory following this naming convention:

```
YYYY-MM-DD-post-title.md
```

For example: `2026-01-23-my-first-post.md`

## Post Template

Copy this template for new posts:

```markdown
---
layout: post
title: "Your Post Title"
date: 2026-01-23
categories: [category1, category2]
---

Your post content goes here in Markdown format.

## You can use headings

- Bullet points
- Lists
- **Bold text**
- *Italic text*

### Code blocks

```python
def example():
    print("Code examples work great")
```

### Links and images

[Link text](https://example.com)
![Alt text](/path/to/image.jpg)
```

## Front Matter Fields

- `layout: post` - Required (uses the post layout)
- `title` - Required (the post title)
- `date` - Required (YYYY-MM-DD format)
- `categories` - Optional (for organizing posts)
- `excerpt` - Optional (custom preview text, otherwise uses first paragraph)

## Categories

Common categories you might use:
- `data-engineering`
- `research`
- `economics`
- `technical`
- `tutorial`
- `update`

## Tips

1. Use descriptive file names (they become the URL slug)
2. Include the date in YYYY-MM-DD format
3. Write in Markdown
4. Save with `.md` extension
5. Preview locally with `bundle exec jekyll serve`
