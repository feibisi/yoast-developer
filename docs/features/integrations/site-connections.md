---
title: "Site Connections"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Integrations
    - Features
    - Repos
---


This documentation provides information about how [Yoast SEO](https://yoast.com/product/yoast-seo-wordpress/) integrates with various search engine's tools and portals.

We provide tools for managing verification codes for Baidu Webmaster Tools, Bing Webmaster Tools, Google Search Console, and Yandex Webmaster Tools.

## Managing verification codes
Each of the above vendors expects a distinct format, and has its own validation rules. We sanitize and validate inputs in line with those.

*Details pending*

## Outputting codes
Each validated input should result in a meta tag (with a distinct format in each case) being added to the `<head>` on the *homepage only*.

*Details pending*