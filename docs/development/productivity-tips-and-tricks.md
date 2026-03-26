---
title: "Productivity Tips And Tricks"
post_status: publish
comment_status: open
taxonomy:
  category:
    - yoast-developer
  post_tag:
    - Development
    - Repos
    - Data
---


以下是几件你可以做或学习的事情，让软件开发工作变得更轻松。这是一个不断增长的列表，欢迎随时补充！

**注意：大部分说明假设你在使用 OSX 系统。**

## PhpStorm

### Keyboard shortcuts

| Shortcut    | Description                                                                                                                                                                                                                                                                                                             |
|-------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `⌘ + e`     | Opens the 'recent files' window. Select by typing the name of the file or moving up and down with arrow keys. Or press the return key directly for the previous file.                                                                                                                                                   |
| `⌥ + ↑`     | Hard to explain, just give it a try... Selects, or extends, the selection of text in your editor. Pressing it repeatedly, increases the scope in which code is selected. Can be used to select a word, a statement or variable, a line, a function or a class and so on.                                                |
| `⌘ + ⌥ + l` | Reformats your code according to what you have setup in your code style settings (`PhpStorm` → `Preferences` → `Editor` → `Code Style` → `{your language}`). A very useful tool for preventing your colleagues from requesting changes on your pull requests just for a code style error.                               |
| `⌘ + 9`     | Opens the 'version control' panel. Here you can check out the history of a branch. View which files are changed in what commit. And albeit with a little more effort, open the changed files in the editor (it defaults to changes window).                                                                             |
| `^ + g`     | [Multiple selection](https://blog.jetbrains.com/phpstorm/2014/03/working-with-multiple-selection-in-phpstorm-8-eap/): select next occurrence. Find the next occurrence of the selection and adds a cursor there too. Very handy for a quick refactor in one file. Also you can just `⌥ + click` to add an extra cursor. |

## Sublime Text

### 实现类似 PHPStorm 的代码自动缩进功能
打开菜单栏中的快捷键设置：`Sublime Text` → `Preferences` → `Keybindings`

这会打开两个文件。编辑右侧的文件（这是用户专属配置文件）。在 `[]` 之间添加这行代码，然后重启 Sublime。
``` json
{ "keys": ["command+option+l"], "command": "reindent", "args": {"single_line": false} }
```

现在 `⌘ + ⌥ + l` 组合键就能正确缩进你的代码了。

## iTerm 2

### 自然文本编辑
在 Mac 上的普通文本编辑器中，您可以使用 `⌘ + ←` 或 `⌘ + →` 快速浏览文本。要在 iTerm 2 中启用此功能，请转到 `Preferences` → `Profiles` → `Keys`。然后，点击 "Presets" 下拉菜单并选择 "Natural Text Editing"。

## Git

### 删除已合并的分支
（此操作仅删除本地分支，不会以任何方式影响远程仓库。）

将以下行添加到你的 `~/.bash_profile` 或 `~/.zshrc` 文件中。仅当你在使用 Z-shell（或 `echo $SHELL` 输出类似 `/bin/zsh` 的内容）时才使用后者。

```shell script
alias delete-merged='git branch --merged | grep -v "\*" | grep -v main | grep -v trunk | grep -v develop | xargs -n 1 git branch -d'
```
现在，你可以在任何 git 仓库的终端中使用 `delete-merged` 命令来删除已合并到 `trunk`、`develop` 或 `main` 分支的分支。

### 别名提示
这是一篇关于 Git 别名的好文章：https://haacked.com/archive/2014/07/28/github-flow-aliases/

## Autojump
Autojump 是一个极佳的工具，能让你在终端中飞速跳转到所需目录。

如果你已安装 brew（强烈推荐安装），运行 `brew install autojump`。然后，如果你使用 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)，只需在 `~/.zshrc` 文件中找到 `plugins` 变量，并将 `autojump` 添加到列表中。重新加载配置文件或重启终端后，即可使用 `j` 命令跳转到安装 autojump 后访问过的任何目录。

### 示例
无需输入 `cd ~/Dev/yoast.com/site/web/app/themes/yoast-com`，您可以直接键入 `j yoast-c`。它仅需路径或目录名的一小部分即可识别目标位置。

若 autojump 选择了错误目录，只需再次运行该命令。此时它将选取次优匹配项。您可通过在该目录下执行 `j -i [权重值]` 来提升当前目录的权重。运行 `j -s` 可查看所有已索引目录的当前权重。

## 命令行
* `pbcopy` 可将特定结果复制到剪贴板
* `pbpaste` 可从剪贴板粘贴最近复制的内容

例如要将 SSH 密钥上传至 GitHub，可使用命令 `cat ~/.ssh/id_rsa.pub | pbcopy` 再粘贴到 GitHub 界面。若需将外部数据导入脚本，`pbpaste` 非常实用，例如 `pbpaste | ./awesome-data-processing-script.php`。

## `sudo` 便捷性
* 延长 `sudo` 超时时间：将此行添加到 `/etc/sudoers` 文件中：
```shell script
Defaults timestamp_timeout=15
```