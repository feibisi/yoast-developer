---
title: "IDE 特定文件 #"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Standards
    - Development
    - Repos
---


由于我们重度依赖 Git 进行版本控制，因此在管理代码时必须执行一套严格的规则。

## 分支
我们的仓库包含几个特殊分支：

| 分支          | 描述 |
|---------------|-------------|
| `main`        | 此分支包含最新发布的版本。它不应包含任何未经适当审查和测试的代码。 |
| `develop` *   | 此分支包含正在积极开发并将于下次发布中上线的代码。创建新分支时，应基于此分支。 |
| `release/*.*` | 此分支在制作发布候选版本时创建，以确保 `trunk` 上的开发工作可以继续进行。 |
| `hotfix/*.*`  | 此分支用于为最新发布版本创建补丁。此分支始终基于 `main`，而非 trunk。

**\* 在某些仓库中，`develop` 仍被称为 `trunk`。这是我们未来将要更改的事项。**

### 例外情况
唯一允许直接提交到 `main` 分支的例外情况是处理高优先级错误修复或安全补丁。一旦计划发布，将使用 `develop` 分支创建候选发布版（RC），并最终合并到 `main` 分支。

My Yoast 项目采用不同的设置，功能分支在满足所有要求后直接合并到 `main` 分支。因此，该代码库中没有 `develop` 分支。

### 创建分支
每当开始处理问题时，务必使用独立分支进行提交。这能确保未完成（或可能存在缺陷）的代码不会扩散到所有开发环境。

这些不同的分支分别承载着以下"真相"：已发布的内容（`main`分支）、可发布的内容（`develop`分支），以及最终将发布的内容（功能分支）。

**请避免从未合并的分支创建新分支，这实际上会产生反效果。若需先合并某分支才能继续处理其他问题，请向团队成员申请代码审查和验收测试。**

#### Branch naming conventions
To ensure branches can easily be identified within a repository, we adhere a convention that follows the following structure:
`{issue number}-{issue title}`.

Generally speaking the issue title is a short description of the problem that needs to be solved.

**This format implies that all PR's should have an issue on which they are based to ensure there's a clear separation of issue and solution.**

In the past we used an alternative convention that also contained your initials, but seeing at multiple people can be working on a single issue, it doesn't make much sense to adhere that structure. You might also run into a structure like `stories/{issue title}`. This is also part of the 'old' way of naming branches.

#### 树状图示例
我们的 Git 树状结构应如下所示：

```
main
  |
  |-123-fix-typo
  |-132-xss-in-metabox
  |-...
  |
  |---trunk
        |
        |-164-add-button
        |-166-twitter-integration
        |-...
```

## 提交
请尽量使用清晰的提交信息，并保持每次提交尽可能精简。

## 拉取请求

#### 准备你的分支
当你的分支准备就绪后，请确保将父分支（`main`、`trunk` 或某个功能分支）的最新版本合并到你的当前分支中。可以通过执行以下命令完成：

```shell script
git checkout {parent_branch}
git pull
git checkout {your_branch}
git merge {parent_branch}
```

你可能会遇到一些合并冲突。解决它们，提交合并，并在提交拉取请求（PR）之前推送代码。

### 提交 PR 前
三条黄金法则：

- 遵循 [Yoast 编码指南和原则](docs/development/standards/coding-guidelines-and-principles)。
- 确保已添加测试（如适用）。这也适用于错误修复。
- 在 PR 中添加步骤，以便验收测试人员了解代码的预期行为。同时包括添加/更改配置文件等事项。

**注意：不要假设测试人员使用与你相同的设置！撰写 PR 时请务必考虑这一点。**

### 创建 PR
提交 PR 时，请确保将您的分支与正确的父分支进行比较。对于某些仓库，默认父分支是 `trunk`，但您可以在页面顶部更改此设置：

![branch_comparison](https://raw.githubusercontent.com/woocommerce/woocommerce/trunk/docs/development/standards/branch-comparison.png)

通过添加 `Fixes #{issuenumber}` 来指明修复了哪个问题。这将确保 PR 合并后问题自动关闭。

您的代码现已准备好进行代码审查。

## 其他

### 全局 Git 忽略
为确保我们不会将不属于仓库的文件提交到任何仓库，Git 提供了一种全局排除文件使其不被暂存提交的方法。

要为 Git 添加全局忽略文件，请在终端中执行以下命令：
```shell script
git config --global core.excludesfile ~/.gitignore_global
```

将以下数据输入到文件 `~/.gitignore_global` 中：
```shell script
# IDE 特定文件 #
.idea

# 编译文件 #
.so

# 操作系统生成的文件 #
######################
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
```

### 配置 Git 钩子

#### 切换分支时自动运行 `composer install`
以下命令创建一个 Git 钩子，该钩子会在执行 `git checkout` 时触发。
这将运行 `composer install` 命令，以确保您拥有正确的包和生成的自动加载器。

```shell script
cat <<EOT >> .git/hooks/post-receive
#!/bin/sh
composer install --working-dir=$GIT_DIR/../
EOT
chmod +x .git/hooks/post-receive
```

您可以将此钩子用于所有使用 Composer 进行依赖管理的仓库。