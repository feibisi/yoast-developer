---
title: "Setup"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Environment
    - Development
    - Repos
---


本页描述了我们在 Yoast 设置开发环境的过程。

:::note
本指南假设您使用 Mac OSX 和 PHP 7.4+ 环境。
:::

## 开始之前
在开始为 Yoast 插件做贡献之前，请确保您已查阅我们的[开发工具页面](docs/development/environment/tools)，其中列出了需要使用的各种工具（除非另有说明）。

## 项目组织建议
通常我们建议创建一个 `~/Projects/Yoast` 文件夹来集中管理所有内容。

:::note
请勿在 `~/Documents`、`~/Desktop` 或 `~/Downloads` 目录下创建 `Yoast` 文件夹，否则在使用 `plugin-development-docker` 等容器化环境时可能引发错误。
:::

创建该文件夹后，您有两种选择：

*   按环境逐个克隆插件，这样可以在系统中保留同一插件的多个版本，避免（可能的）冲突。
*   将所有插件克隆到单一目录，并通过符号链接关联到各开发环境。这样所有关联的开发环境都将运行相同版本（分支）的插件。

:::note
通常更推荐使用符号链接方案，因为单一数据源能降低操作失误的风险。
:::

### 按环境逐个克隆插件
如果您选择使用 [Local](https://localwp.com/) 作为开发环境，可以按照以下步骤将插件克隆到 Local 站点的插件目录中。

:::note
这些步骤假设您已经[安装了 Local，并在其中设置了用于插件开发的站点](docs/development/environment/tools#local)。
:::

#### 在 Mac 上
* 将您想要开发的插件克隆到 `~/Applications/Local\ Sites/<site-name>/app/public/wp-contents/plugins` 目录下，其中 `<site-name>` 是您想用于插件开发的 Local 站点名称。

### 将插件克隆到单一目录并创建符号链接
如果您使用 Local 作为首选开发环境，可以按照以下步骤将插件克隆到独立目录，并创建指向 Local 站点插件目录的符号链接。

#### 在 Mac 上
* 将你想要开发的插件克隆到一个选定的文件夹中。
* [将此文件夹创建符号链接](https://www.howtogeek.com/297721/how-to-create-and-use-symbolic-links-aka-symlinks-on-a-mac/)到 `~/Applications/Local\ Sites/<site-name>/app/public/wp-contents/plugins` 文件夹，其中 `<site-name>` 是你想用于插件开发的 Local 站点名称。

## 设置 PHPUnit

### 在 PhpStorm 中启用 PHPUnit
在 PhpStorm 中完成设置，请遵循以下步骤：

*   在 PhpStorm 的 `Run` 菜单下，转到 `Edit configurations`。
*   在接下来的窗口中，点击左上角的 `+` 号，并在选项中选择 PHPUnit。
*   选择选项：`Defined in the configuration file`，并勾选 `Use alternative configuration file` 的复选框。
*   在复选框后面，输入 `phpunit.xml.dist` 文件的完整路径。该文件位于插件目录中。如果代码库包含 `phpunit.xml` 文件，请改用该文件。（例如，`wordpress-seo` 有两种类型的测试。对于“旧版”集成测试，使用 `phpunit-integration.xml.dist`；对于 BrainMonkey 测试，使用 `phpunit.xml.dist`。）
*   输入路径后，点击最右侧的图标。这将带你进入 `Test frameworks` 窗口。
*   点击加号图标并选择第一个选项：`PHPUnit Local`。选择 `Use Composer autoloader`，这将自动填充 `Path to script` 为一个以 `/vendor/autoload.php` 结尾的路径。此选择将提示 PhpStorm 使用通过 Composer 获取的版本。
*   最后，当你返回到 `Run/Debug configurations` 窗口时，底部可能会出现一条错误信息。点击旁边的 `Fix` 按钮，并选择 PHP 作为你的 CLI 解释器。应用并完成！

### 配置 PHPUnit 以配合 WordPress 和插件工作
为确保单元测试能够正常运行，您需要添加以下两个常量：

* `WP_DEVELOP_DIR` - 指向您系统中 `wordpress-develop` 仓库的克隆路径。
* `WP_PLUGIN_DIR` - 指向 WordPress 插件所在的目录。

示例：

```
WP_DEVELOP_DIR=/Users/<your name>/Projects/WordPress/wordpress-develop/
WP_PLUGIN_DIR=/Users/<your name>/Projects/Yoast
```

:::caution
请注意 `WP_DEVELOP_DIR` 末尾的斜杠，而 `WP_PLUGIN_DIR` 则没有。
:::

:::info
如果您已将 `WP_DEVELOP_DIR` 和 `WP_PLUGIN_DIR` 设置为环境变量，可以跳过以下部分。
:::

在 PhpStorm 中配置 PHPUnit：转到 `编辑配置...` -> `PHPUnit`，并确保配置与以下截图类似：

  ![](https://lh5.googleusercontent.com/9TJaufyDOzjcM9bGn6ELSVnGTL6tfFOlW8LMoTmmOLoOtvANfN36B5kVZ72iesKy8isliFQHmSq2uMPR58FkHwRVqPCF_O9MvdEAhy4QHK1h53Kp6ppiJ83d70AUNLOSFBDvuxyv)

### 运行多站点测试
我们部分插件包含仅在多站点环境下运行的测试。要执行这些测试，请选择您的 PHPUnit 配置，并在设置界面的命令行部分添加值为 `1` 的 `WP_MULTISITE` 参数。

## 安装插件
现在开始安装插件。首先，如果你还没有操作，请在 `/plugins/` 文件夹中克隆 [wordpress-seo 仓库](https://github.com/Yoast/wordpress-seo)。

然后，在你克隆仓库的目录中运行以下命令：

* 运行 `composer install` 以确保安装所有 PHP 依赖项。
* 运行 `nvm use` 来设置正确的 Node 版本。
* 运行 `yarn` 来安装 JavaScript 依赖项。
* 运行 `grunt build` 来手动构建 JavaScript（和 CSS）。

之后，你可以运行 `yarn start` 来监听变更并自动构建 JavaScript。

### 其他插件

我们还提供了其他几个插件。所有公开可访问的代码库都可以[在 GitHub 上找到](https://github.com/Yoast/)。

## 启用可索引化索引
* 安装 [Yoast 测试助手插件](https://wordpress.org/plugins/yoast-test-helper/)。
* 在 Yoast 测试助手的设置中启用“开发模式”。这将在开发站点上启用可索引化索引功能。

## 为测试生成模拟数据
您可以使用 [Yoast WP CLI Faker](https://github.com/Yoast/wp-cli-faker) 来生成模拟数据，例如文章、分类和用户。

* 利用 Local by Flywheel 中的 _打开站点 Shell_ 按钮。
* 按照 [_作为 WordPress 包安装_ 的说明](https://github.com/Yoast/wp-cli-faker#install-as-wordpress-package) 来获取该包。
* 按照 [_核心_ 说明](https://github.com/Yoast/wp-cli-faker#core) 来生成模拟数据。
  * 注意，对于多站点，您可以使用 `--url=<url>` 参数来定位特定的子站点。
    * （请参阅 [如何在 WordPress 多站点上对一个或多个站点运行 WP-CLI 命令](https://danielbachhuber.com/tip/run-wp-cli-command-wordpress-multisite/)。）
* 可选：按照 [_WooCommerce_ 说明](https://github.com/Yoast/wp-cli-faker#woocommerce) 为 WooCommerce 生成模拟数据。
* 要退出 Shell，请使用 `exit`。

## 接下来做什么？
克隆仓库后，你可以在浏览器中访问 `basic.wordpress.test` 查看开发环境。在 IDE 中打开插件目录，就可以开始开发了！