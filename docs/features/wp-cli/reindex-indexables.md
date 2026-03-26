---
title: "Reindex Indexables"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Wp Cli
    - Features
    - Repos
---


随着 Yoast SEO 14.0 的发布，我们引入了新的数据库表，用于整合站点中所有可索引对象的元数据。
然而，这些数据库表需要先填充数据才能正常使用。为了在大型站点上高效完成此操作，我们引入了 WP CLI 命令。
本页将介绍该命令及其所有选项。

:::tip
如果您不了解 WP CLI，那您就错过了一个好工具！它是 WordPress 的命令行界面，能大大简化许多任务。[在 wp-cli.org 上了解更多信息](https://wp-cli.org/)。
:::

如果您计划使用我们的 [REST API](/customization/apis/rest-api) 或 [界面 API](/customization/apis/surfaces-api)，提前在您的网站上运行 CLI 命令非常重要。

## 语法

运行索引化过程的基本命令如下：

```shell script
wp yoast index
```

```shell script
索引文章  100% [==============================] 0:00 / 0:00
索引分类项  100% [==============================] 0:00 / 0:00
索引文章类型归档页  100% [=================] 0:00 / 0:00
索引通用对象  100% [====================] 0:00 / 0:00
```

### 注意事项

* 如果其中某项未显示，是因为其中的所有项目都已被索引。
* 作者信息会在文章索引过程中一并索引。
* 如需多次测试此索引命令，请使用 [Yoast Test Helper](https://wordpress.org/plugins/yoast-test-helper/)。点击该插件中的“重置可索引表与迁移”按钮，
将删除可索引表。之后您可以重新运行索引过程。

## 选项

运行索引命令时还有一些可选参数：

* `--network`：对网络中的所有站点执行索引操作。
* `--reindex`：删除所有现有可索引数据并重新索引。

## 故障排除

### Index 不是 'yoast' 的已注册子命令

这意味着您使用的 Yoast SEO 版本不正确。请注意，Yoast SEO WP CLI 索引命令需要 Yoast SEO 14.0 或更高版本才能正常工作。