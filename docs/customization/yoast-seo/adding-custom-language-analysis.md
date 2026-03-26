---
title: "Adding Custom Language Analysis"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Yoast Seo
    - Customization
    - Repos
---


我们的软件支持多种语言。如果您希望添加自定义语言分析，分析文件需要在 window 对象上可用。有两种方法可以引入正确的语言分析 JS 文件：

1. 自行创建自定义语言文件并将其分配给 window 对象。例如：`window.yoast.researcher = specific researcher;`
2. 按照我们的方式配置 webpack；这样 webpack 会为您生成文件。