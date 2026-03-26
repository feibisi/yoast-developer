---
title: "Phpunit Best Practices"
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


除了按照特定的编码指南和原则编写优质代码外，在编写测试时遵循某些准则也同样重要。

## 理解基础编码原则

在开始之前，充分理解一些基础编码原则非常重要，例如面向对象编程（OOP）、SOLID 原则和 DRY 原则。

请阅读我们的[编码指南与原则](https://developer.yoast.com/development/standards/coding-guidelines-and-principles/)部分以获取更多信息。

## 我们如何组织 PhpUnit 测试

Yoast 的大多数 PHP 代码库都采用面向对象的方式编写。这些代码库包含大量类，并且为每个类都创建了一个测试类。
这些测试类通过测试方法来覆盖我们想要测试的类。

### 每个测试方法对应一种情况

编写测试方法时，一个好的做法是每个测试方法只包含一个断言。此外，添加 `@covers` 注解并跟上类名和方法名，可以告诉 PhpUnit 正在测试哪个方法，这之后可用于计算测试覆盖率。

示例：

```php
/**
 * @covers ClassName::MethodName
 */
public function test_method_name() {
    $myClass = new MyClass();
    $this->assertTrue( $myClass->MethodName() ) ; 
}
```

## 使用数据提供器来减少重复测试

有时，同一个类方法有多个测试方法。这不一定是个问题，但你可以通过使用数据提供器将它们合并到一个测试方法中。

数据提供器是测试对象中的一个方法，你可以在其中为多种情况指定不同的值。
提供器总是返回一个数组，其中包含一个子数组，该子数组存放测试值。这些测试值作为参数传递给测试方法。

```php
/**
 * @dataProvider myDataProvider
 * @covers ClassName::MethodName
 */
public function test_method_name( $expected, $valueToTest, $message ) {
    $actual = $myClass->MethodName( valueToTest )

    $this->assertEquals( $expected, $actual, $message ) ; 
}

/**
 * 测试方法 test_method_name 的数据提供器

 * [0]: 期望值
 * [1]: 传递给方法的值
 * [2]: 传递给断言的消息。
 */
public function myDataProvider( ) {
   return [
       [ true, ‘MyMethod’, ‘Test 1: The string is MyMethod’ ],
       [ false, ‘NotMyMethod’, ‘Test 2: The string is not MyMethod’ ],
   ];
}
```
`[0]` 期望值是你可以在数据提供器中添加的内容。这样做可以阐明提供器中数据的结构。

消息是可选的，但使用它们可以帮助使 PHPUnit 中的断言更清晰一些。通过在数据提供器中提供消息，将更容易识别失败的测试。