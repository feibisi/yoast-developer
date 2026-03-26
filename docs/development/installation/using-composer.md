---
title: "Using Composer"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Installation
    - Development
    - Repos
---


## 什么是 Composer？
[Composer](https://getcomposer.org/) 是一个用于 PHP 项目的依赖管理工具（类似于 NPM），可以通过终端运行。

## 安装 Composer

要在您的系统上安装 Composer，请确保遵循[官方安装步骤](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos)。

## 通过 Composer 安装 Yoast SEO
要通过 Composer 安装 Yoast SEO，请在您的 `plugins` 目录中运行以下命令：

```shell script
composer require yoast/wordpress-seo
```

上述命令将下载最新版本的 Yoast SEO 插件并安装所有必要的依赖项。

## 安装 Yoast SEO 的开发版本
如果您希望帮助开发 Yoast SEO（例如修复错误），我们建议您使用 `git clone git@github.com:Yoast/wordpress-seo.git`，因为我们不支持通过 Composer 下载 Yoast SEO 的开发版本。

## 安装后步骤
一旦 Composer 完成其安装过程，或者 git 完成克隆仓库，请确保在 `wordpress-seo` 目录中运行 `yarn`，然后运行 `grunt:build`，以确保所有必要的文件都已正确构建。

## 更新依赖项

Composer 在 `composer.lock` 文件中跟踪项目的状态。运行 `composer install` 将始终重现该状态，确保不同开发者环境的一致性。
当需要更新依赖项时，以下命令将根据 `composer.json` 中设置的版本限制，将相关依赖项更新到最新版本。

```shell script
composer update
```

之后，更新后的 `composer.lock` 文件需要提交到版本控制中。

:::caution

请注意，更新依赖项是一个需要谨慎处理的过程，这样做可能会导致功能损坏。
在提交更新后的 `composer.json` 和 `composer.lock` 版本之前，请务必进行充分的测试。

:::