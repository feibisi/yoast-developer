---
title: "Setup Plugin Integration Tests"
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


要运行我们任何 WordPress 插件的集成测试，你需要满足以下条件：
1. 能够访问你想要测试的 PHP 版本。
2. 一份包含基础测试用例的 `wordpress-develop` 仓库副本。
3. 一个带有空数据库的 MySQL 服务器。

## 安装 PHP
如果尚未安装，您可以按照 [PHP 安装文档](https://www.php.net/manual/en/install.php) 在您的机器上安装 PHP。
如果因任何原因无法/不想在本地安装 PHP，您也可以选择使用 Docker 容器运行测试，但该过程本文档不作说明。

## 克隆 `wordpress-develop` 仓库
克隆 [WordPress/wordpress-develop](https://github.com/WordPress/wordpress-develop)。
集成测试会在您启动测试时，从这个克隆仓库加载 WordPress 及其所有函数和 API。这意味着我们可以针对您本地安装的 WordPress 版本进行测试，这正是“集成”部分。
它还为我们的测试提供了一些实用类（例如，测试用例）。

## 设置 MySQL 服务器
有多种方式可以满足此要求。这里我们概述了 Yoast 团队喜欢使用的几个选项。您可以根据个人需求/偏好选择其中之一。

### 选项 1：使用 wordpress-develop Docker 环境提供的 MySQL 服务器
此选项的额外好处是，您还可以获得一个用于为 WordPress 本身编写补丁和单元测试的独立设置。

#### 暴露 Docker MySQL 实例
我们将本地暴露 Docker MySQL 实例，以便 PHPUnit 测试可以访问它。

#### 暴露 MySQL 端口
在你之前克隆的 `wordpress-develop` 本地副本中，你需要暴露 MySQL 端口，这将允许我们稍后连接到它。

在 `wordpress-develop` 目录中，添加一个 `docker-compose.override.yml` 文件，并设置以下内容：

```yaml
services:
  mysql:
    ports:
      - "3306:3306"
```

这将把 3306 端口暴露给本地环境。

#### 编辑 hosts 文件
在终端中运行 `sudo nano /etc/hosts` 来编辑你的 hosts 文件，并添加以下内容：

```shell script
127.0.0.1 localhost
10.254.254.254 mysql
```

#### 使数据库可访问
为了使你的数据库可访问，请运行以下命令：

* 在 Mac 上：
```shell script
sudo ifconfig lo0 alias 10.254.254.254
```

* 在 Linux 上：
```shell script
sudo ifconfig lo:0 10.254.254.254
```

* 在 Windows 上：
```shell script
netsh interface ipv4 add address "Loopback Pseudo-Interface 1" 10.254.254.254 255.255.255.255
```

#### 启动容器
如果您已完成上述所有步骤，现在可以启动容器了！

在终端中，进入 `wordpress-develop` 目录，运行以下命令：

```shell script
npm install
npm run build:dev
npm run env:start
npm run env:install
```

当您想停止容器时，只需执行：

```shell script
npm run env:stop
```

### 选项 2：使用 Local by Flywheel 开发环境提供的 MySQL 服务器
如果你使用 Local 进行开发，这可能是一个简单的替代方案。

1.  在 Local 中启动你的开发站点。
2.  切换到 "Database" 选项卡。
3.  在这里，你应该能看到一个 "Open Adminer" 链接。点击它。
4.  在 Adminer 中，靠近左上角有一个 "SQL command" 链接。点击它。
5.  在文本区域输入 ``CREATE DATABASE `plugins-integration-test` `` 并点击 "Execute"。

现在你已经有了数据库，接下来需要告诉测试套件如何连接到它：

1.  在你的 `wordpress-develop` 克隆仓库中，找到 `wp-tests-config.php` 文件。如果它还不存在，请复制 `wp-tests-config-sample.php` 并将其命名为 `wp-tests-config.php`。
2.  找到如下所示的代码块：
	```
		define( 'DB_NAME', 'youremptytestdbnamehere' );
		define( 'DB_USER', 'yourusernamehere' );
		define( 'DB_PASSWORD', 'yourpasswordhere' );
		define( 'DB_HOST', 'localhost' );
		define( 'DB_CHARSET', 'utf8' );
		define( 'DB_COLLATE', '' );
	```
3.  将 `DB_NAME` 的值改为 `plugins-integration-test`。
4.  将 `DB_USER`、`DB_PASSWORD` 和 `DB_HOST` 的值更改为你的 Local by Flywheel 站点 "Database" 选项卡中列出的值。

## Tying it all together
The last step is to tie it all together! Open the folder where you cloned `wordpress-seo`.

In order to ensure that unit tests can properly run, you need to add the following two constants:

* `WP_DEVELOP_DIR` - Refers to the cloned repository of `wordpress-develop`, as it's located on your system.
* `WP_PLUGIN_DIR` - Refers to the directory where the WordPress plugins are located.

You can add those to the `phpunitxml.dist` file in the root of the `wordpress-seo` repository.
If you followed our earlier recommendations your will be adding the constants as below:

```xml
  <php>
    <env name="WP_DEVELOP_DIR" value="/Users/<your name>/Projects/WordPress/wordpress-develop/" />
    <env name="WP_PLUGIN_DIR" value="/Users/<your name>/Projects/Yoast" />
  </php>
```

:::caution
Note the trailing slash in `WP_DEVELOP_DIR` and the absence of it in `WP_PLUGIN_DIR`.
:::

Now, you can (finally!) run the integration tests via:

```shell
composer test-wp
```

That's all! Congratulations! You now have a working setup to run the integration tests for the Yoast SEO plugins.

## 故障排除

### MySQL 连接冲突

如果遇到数据库连接错误，请确保没有其他 MySQL 进程在后台运行。可以在终端中执行 `brew services stop mysql` 命令来停止 MySQL 服务。

### 访问管理后台（wordpress-develop docker）

Docker 设置实际上运行的是一个完整的本地 WordPress 站点。出于调试目的，您可以访问此测试环境的管理后台。默认情况下，访问 WordPress 管理后台的凭据是：

* 主机 - `http://localhost:8889/wp-admin`
* 用户名 - `admin`
* 密码 - `password`

### 访问数据库（wordpress-develop docker）

如需访问数据库以检查某些内容（例如是否已创建所有数据库表），可在您选择的数据库工具中使用以下凭据：

* 主机 - `10.254.254.254`
* 用户名 - `root`
* 密码 - `password`
* 数据库 - `wordpress_develop`
* 端口 - `3306`