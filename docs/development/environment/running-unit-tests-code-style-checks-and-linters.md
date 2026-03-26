---
title: "Running Unit Tests Code Style Checks And Linters"
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


在 Yoast，我们使用多种工具来确保代码遵循特定标准，这让我们能更有信心地发布产品并减少错误。

这些工具包括：

* 单元测试 - 确保独立函数/方法按预期运行。
* 集成测试 - 确保一系列操作（即函数/方法调用）按预期运行。
* 代码检查工具 - 确保代码以统一方式编写（即结构、间距等）。

## PHP

### 单元测试
针对我们的 PHP 代码，我们使用 PHPUnit 来测试代码是否符合预期行为。在运行任何测试之前，请确保您已阅读 [PHPUnit 设置指南](docs/development/environment/setup#set-up-phpunit)。

同时请务必阅读关于 [在多站点环境中运行测试](docs/development/environment/setup#running-multisite-tests) 的部分。

#### Running unit test with code coverage
Running unit tests with code coverage is very similar to 'just' running unit tests, with one difference: While a test runs, all your code gets analyzed to see what parts of your code base are covered by the tests.

The results of this analysis are then stored as a report that can later be used to determine what parts of your code might need more tests. Please note that running unit tests with code coverage takes up more time, as analyzing your code requires extra resources.

To configure your system to be able to run with code coverage, follow these steps:

1. Run `pecl install xdebug` in your terminal to install XDebug.
2. To run the tests including the coverage, click <img alt="Coverage button in PhpStorm" src="https://user-images.githubusercontent.com/17744553/53946611-714ab580-40c4-11e9-85b6-fde5576e4609.png" /> in the upper right corner of PhpStorm.

##### Troubleshooting

**XDebug won't load**

If you get an error `"Failed loading /usr/local/Cellar/php@7.1/7.1.26/lib/php/20160303/xdebug.so"` or something similar, it is possible your extension directory needs to be configured so PHP knows where to look for it.

Follow these steps to configure your extension directory:

1. Run `php --ini` in your terminal to find your `php.ini` file.
2. Open the `php.ini` file in your preferred editor.
3. To find out what the proper extension directory is, run: `pecl config-get ext_dir` in a separate terminal window.
4. Now uncomment and update the extension_dir in your php.ini:`extension_dir = /usr/local/lib/php/pecl/<php_api_version>` and replace the pathname with the one you found in the previous step.

Sometimes the unit tests will run on a different PHP version than the one used for code coverage. This is because of a quirk in PhpStorm.

To fix this, under `Edit Configurations...` -> `Command line` -> `Interpreter`, you can specify an interpreter. However, running with code coverage always uses the `<Default project interpreter>`, even when you have specified another interpreter. In case you get fatal errors when running with code coverage, take a look at the default project interpreter (`Preferences` -> `Languages & Frameworks` -> `PHP`) and ensure that this one is also properly configured.

**XDebug doesn't have the right SSL permissions**

If you get an error regarding SSL permissions, please run `brew install openssl` and try to install XDebug again.

### 集成测试
如果您对搭建和运行集成测试感兴趣，请务必查阅我们的指南：[为插件设置集成测试](docs/development/environment/setup-plugin-integration-tests)。

#### 其他插件
我们的大多数其他插件也包含各种测试，用于检查 PHP 代码。要运行这些测试，请在终端中使用以下命令：

* `composer test`

### 代码检查

#### 检查 PHP 语法错误
**免费版**

* 使用 `composer lint`

**高级版**

* 使用 `composer premium-lint`

**其他方法**

要检查语法错误，请运行 `find -L . -path ./vendor -prune -o -path ./node_modules -prune -o -name '*.php' -print0 | xargs -0 -n 1 -P 4 php -l`

#### 编码规范
在我们的代码库中使用代码检查工具时，我们综合采用了以下多种编码标准：

* WPCS
* PHPCompatibility
* [YoastCS 检测规则](https://github.com/Yoast/yoastcs)

#### 运行代码检查工具
**免费版**

* 如需使用交互式菜单，请执行：`composer cs`
* 若要将当前分支文件与 `trunk` 以外的分支进行对比检查，请执行：`composer check-branch-cs {BRANCH-TO-CHECK-AGAINST}`。参数应为目标合并分支的名称。

**高级版**

* 免费版的所有命令同样适用于高级版。

**新闻模块**
* 全面检查代码：`grunt phpcs`

**视频模块**
* 全面检查代码：`vendor/bin/phpcs`

**WooCommerce 模块**
* 全面检查代码：`grunt phpcs`

## JavaScript

### 单元测试
对于我们的 JavaScript 代码，我们使用 Jest 和 Jasmine 来测试代码的预期行为。请确保在尝试运行以下命令之前已运行 `yarn`，以便安装所有必需的 NPM 包。

要运行测试，请在终端中使用以下命令：

* `yarn test`

#### 覆盖率
要运行测试（包括覆盖率），请在终端中使用以下命令：

* `yarn test --coverage`

### 代码检查

#### 自动运行代码检查工具
要启用此自动功能，请通过以下路径在设置中选择 `Automatic ESlint configuration` 选项：

`PhpStorm` -> `Preferences` -> `Editor` -> `Inspections` -> `JavaScript` -> `Code quality tools` -> `ESLint`

#### 手动运行代码检查工具
要在终端中运行代码检查工具，请使用以下命令：

* `yarn lint`

## 可用性
|                  | wordpress-seo | wordpress-seo-premium | wpseo-news | wpseo-video | wpseo-woocommerce |
|------------------|---------------|-----------------------|------------|-------------|-------------------|
| PHPUnit          | ✅             | ✅                     | ✅          | ✅           | ✅                 |
| PHPLint          | ✅             | ✅                     | ✅          | ✅           | ✅                 |
| PHPCS            | ✅             | ✅                     | ✅          | ✅           | ✅                 |
| JavaScript 测试  | ✅             | ✅                     | ❌          | ❌           | ❌                 |
| ESLint           | ✅             | ✅                     | ❌          | ✅           | ❌                 |