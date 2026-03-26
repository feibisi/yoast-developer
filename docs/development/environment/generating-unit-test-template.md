---
title: "Generating Unit Test Template"
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


`src` 文件夹中的每个类都应在 `tests/unit` 文件夹中有一个对应的单元测试。由于设置新的单元测试文件需要一些样板代码，我们创建了一个方便的工具来为您生成单元测试模板。

## 使用单元测试模板生成器
您可以通过在插件仓库的根目录中调用以下 Composer 脚本来创建单元测试模板：
```shell
composer generate-unit-test 'Fully\Qualified\Class\Name'
```
此命令需要一个必需参数：您要为其生成单元测试模板的类的完全限定类名。请注意，此完全限定类名应放在引号内，以确保命令行能正确处理反斜杠。

## 调用生成器的结果
调用生成器将在 `tests/unit` 目录中创建一个新的单元测试文件。该文件包含一个基本的单元测试。其设置方法会创建被测试类及其模拟依赖项的实例，并将它们都设置在单元测试类上，以便于即将实现的测试用例使用。

## 常见异常及解决方案

### [ErrorException] file_put_contents(...php): failed to open stream: No such file or directory
生成器目前尚不支持在单元测试文件夹中创建目录。如果您在 `src` 文件夹的新目录中添加了类，请确保在单元测试文件夹中也创建了同名的目录。

### [RuntimeException] 单元测试已存在于路径 "tests/unit/..."
这意味着指定路径已存在该类的单元测试。您可以将新的测试用例添加到错误消息中给出的路径对应的文件中。

### [ReflectionException] Class YoastWPSEO... does not exist
请注意错误消息中的类名缺少反斜杠。

这很可能意味着完全限定类名没有放在引号内。请将类名放在引号内，以确保命令行能正确处理其中的反斜杠。

### [ReflectionException] Class Yoast\WP\SEO\... does not exist
这意味着找不到要为其生成单元测试的给定类。请确保您提供了完全限定类名，即类名前加上其命名空间。同时请确保您已在 `src` 文件夹中创建了您的类。