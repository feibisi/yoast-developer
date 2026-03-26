---
title: "Yoast 开发者文档"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Repos
    - Data
    - Docs Translate
---

# Yoast 开发者文档

本仓库包含托管在 [developer.yoast.com](https://developer.yoast.com) 上的所有文档。
所有文档均使用 Markdown 编写，并由 [Docusaurus](https://docusaurus.io) 解析，以用户友好的方式呈现。

## 一些注意事项

### 请务必...
* ...确保文件始终包含正确的 Docusaurus 页眉。否则，文件将无法正常显示。
* ...检查文件是否按逻辑（例如类别或功能）分组，以确保用户可以轻松找到信息。
* ...使用标题。
* ...如果可能，将大段文本拆分到较小的文件中。
* ...在提交 PR 前测试（新添加的）链接，以确保我们避免 404 错误。
* ...使用[提示框](https://docusaurus.io/docs/markdown-features/admonitions)来展示提示、重点和警告信息。

### 请勿...
* ...在项目运作方式不明确时，犹豫是否寻求帮助。

## 贡献

发现拼写错误？想要补充缺失的文档？欢迎 Fork 此仓库，进行所需修改并提交 Pull Request。

## 安装

克隆此仓库后，请导航至仓库所在目录并运行：

```
$ yarn
```

就这样！您已准备好在本地机器上使用该门户。

此后，您有以下选项：

### Local Development
To quickly test docs changes, just run:

```shell
yarn start
```

This command starts a local development server and open up a browser window. Most changes are reflected live without 
having to restart the server.

**Please note:** If you don't want to keep reopening a new tab, ensure you run `yarn start --no-open`. You can also 
combine this command with `BROWSER=<your preferred browser>` to force Docusaurus to open in a different browser than 
Chrome.

If you want to test a bit more thoroughly, run a full build, and serve that locally.

### 生产与开发构建
只需运行：

```shell
yarn build
# 然后本地测试：
yarn serve
```

此命令将静态内容生成到 `build` 目录中，可通过任何静态内容托管服务进行部署。

**注意：** 当存在损坏的内部链接时，构建将失败，因此在提交前运行此命令进行测试是个好主意。

### 搜索
搜索功能依赖于这个 [Docusaurus 本地搜索包](https://github.com/easyops-cn/docusaurus-search-local)，并且仅在您完成完整构建后生效，因为它需要构建搜索索引。

### 添加重定向规则
要添加重定向规则，请将其添加到 [_redirects](_redirects) 文件中。使用的模式是 Cloudflare Pages 的[重定向格式](https://developers.cloudflare.com/pages/platform/redirects/)，该格式支持通配符和占位符。

### 部署
部署过程完全自动化，因此无需配置或用户交互。

## 为 Yoast 开发者门户做贡献
您好！非常感谢您愿意为 Yoast 开发者门户做出贡献。门户的开发工作主要在 GitHub 上进行。

主要有两种贡献方式：
1. [参与问题讨论](#contributing-to-issues)
2. [贡献代码](#contributing-to-code)

### 参与问题讨论
我们使用 GitHub 来跟踪需要讨论和完成的文档公共变更与改进。在创建问题前，请确保您想提出的内容尚未在其他问题中提及。

请在问题描述中尽可能详尽且简洁，如有必要，可添加截图或视频录屏以帮助更好地理解问题。