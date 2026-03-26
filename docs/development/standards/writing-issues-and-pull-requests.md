---
title: "Writing Issues And Pull Requests"
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


团队协作中一个非常重要的方面是恰当的沟通。这不仅限于团队成员，还包括所有相关人员（例如客户、经理、开发人员等）。
为了尽可能提高效率，我们在 Yoast 遵循一些关于编写问题和拉取请求消息的规则。

## 问题反馈
撰写问题反馈时需注意几个重要方面，这是我们尽力遵循的原则。通常，在向任一代码库提交问题时，您可以参照这些步骤，但有时问题可能不适用于模板。更多相关内容将在后文说明。

### 标题
撰写标题时应遵循以下规则：
* 不使用 [功能]、[Bug] 等标签，GitHub 为此专门提供了标签功能
* 清晰描述问题内容。例如："重定向在目标有效时仍抛出 404 错误"，避免使用"重定向失效"这类无法准确概括问题的表述

### Content
The content of the issue is based on a standard template that has been added to most repositories. If you notice a repository without this template, please inform a member of the Plugin team.

In case a template is lacking, please make sure to add the following:

For bug reports:
* Describe what you expected to happen.
* Describe what actually happened.
* Describe the steps to reproduce said issue.
* Add what version of WordPress and Yoast SEO you are using. If relevant, also include what other third-party plugins are installed.

For features / enhancements:
* Describe the feature /enhancement you want to be added. Adding a motivation as to why you want it included, and adding clear specifications, you help the developer that will be working on said request by adding more context to the whole story. A request to add more / better tests is also considered an enhancement and should be labeled as such.

A few general notes regarding writing issues / feature requests:
* The wording needs to be directive.
* Adding screenshots is encouraged (when applicable).
* Add proper labels. If it refers to a specific component, please make sure you apply the applicable label. Also don't forget to label the status of the issue (i.e. `development`, `needs-codereview` etc.).

## 拉取请求
拉取请求（PR）与议题略有不同。以下是撰写 PR 时需要注意的事项。

### 标题
议题的标题描述问题，而 PR 的标题描述解决方案。除此之外，其规则与议题标题相同。

### 内容
对于 PR 的内容，你需要确保添加以下内容：
* 更新日志条目。该更新日志条目应使用现在时态书写（更多信息请参阅[动词时态](https://en.oxforddictionaries.com/grammar/verb-tenses)）。
  请不要回避添加长而描述性的文本，以清晰传达代码的功能。即使 PR 仅包含不面向用户的代码，它仍然对质量保证有用，并可能具有历史价值。
  
    示例：
    * 修复了 [...] 的错误。
    * 引入了 [...] 的按钮。
    * 移除了 [...] 的代码。

* 在 'monorepo' 上提交 PR 时，更新日志条目前需要额外信息，因为该仓库包含多个包。这在 PR 模板中的更新日志条目上方的注释中有说明。
    * 请注意 `[yoast-components]` 和 `[@yoast/components]` 之间的区别，后者是新的。它们不是同一个包。
    * 请为每个包和/或更改添加单独的更新日志条目。如果它们描述相同的更改，使用相同的文本是可以的。
    * 灵感可参考[维护更新日志](https://keepachangelog.com/en/1.0.0/)。
* 更新日志标签。这用于指示更改的类型（对于 monorepo，请参阅[更改类型](https://keepachangelog.com/en/1.0.0/#how)）。
    * 'user-facing' 标签的使用取决于 PR 的目标产品。对于插件，这是最终用户。对于 monorepo，这是库用户，这意味着 PR 可以稍微技术性一些。
* 里程碑。这很重要，这样我们就不会错过更新日志中的任何测试路径和信息。
    * 在正常的工作流程中，可以在合并 PR 时添加里程碑。对于功能分支（或类似情况），你可以在创建 PR 时添加里程碑。
    * 如果所需的里程碑不存在，你应该创建它。
    * 当你将 PR 合并到功能分支时，应该为该功能分支设置一个里程碑。
    * 当你将 PR 合并到 trunk/develop 时，你应该知道这段代码将在哪个版本中发布。一般来说，当合并到 trunk/develop 时，这往往是即将发布版本 _之后_ 的版本。
* 相关的技术选择在适用时需要命名。这允许你分享为什么决定使用特定解决方案的背景。此条目主要适用于包含大量更改或引入大量新代码的 PR。

* 测试步骤。这是重要的一项；如果你不添加测试步骤，其他开发人员将无法重现旧的和新的行为，从而验证你的代码是否实际有效。请尽可能明确。这包括提及诸如测试前可能需要更改的特定设置，或需要安装的其他特定插件/主题等内容。更多信息见下文！

*   最后，添加 `Fixes #<issue_id>` 这一行非常重要，因为它将 PR 链接到相应的 GitHub issue，以便在 PR 合并后自动关闭。对于 Jira 上报告的问题，应使用以下格式：`Fixes [<issue ID>]`。

#### 编写测试说明
每当您提交 PR 时，必须同时包含适当的测试说明。这不仅是因为开发者会审阅，测试人员（如 SEO 和 Beta 测试员）也需要检查问题是否已正确修复。

提交 PR 时请确保描述以下内容：
* 简要背景介绍——不要假设测试人员完全了解插件内的所有动态组件。若希望测试人员在测试前确认某些功能正常运行：请说明他们需要检查的内容。
* 仅包含相关信息——我们仅支持当前版本的插件，且必须向后兼容至两个版本的 WordPress。
* WordPress 支持——仔细检查最近的 WordPress 更新是否影响我们的修改。如有影响，请在 PR 中注明，以便测试人员了解不同 WordPress 版本间可能存在的差异。
* 自定义设置——若测试需要自定义文章类型或用户权限等，请确保提供测试人员可用的解决方案链接（插件、代码片段等）。