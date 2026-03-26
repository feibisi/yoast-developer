---
title: "START YOAST BLOCK"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Robots Txt
    - Features
    - Repos
---


This documentation explains how [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/) modifies robots.txt files.

## Default content
When a site does not have a user-defined `robots.txt` file (at the point of installation/activation), Yoast SEO generates a default output.

```
# START YOAST BLOCK
# ---------------------------
User-agent: *
Disallow:

Sitemap: https://www.example.com/sitemap_index.xml
# ---------------------------
# END YOAST BLOCK
```

If a `robots.txt` file already exists and contains non-empty rules, this block should be prefixed before any existing rules.

## Controls
Users must be able to edit the contents of their `robots.txt` file.