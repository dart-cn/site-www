---
layout: guide
title: "Dart 语法预览"
description: "帮你了解 Dart 语言的主要特性。"
short-title: 语法预览
---

本页内容告诉你如何使用 Dart 语言的主要特性，从变量到操作符、
从类到库，我们假定你在阅读本页内容之前已经
了解过其他编程语言了。

关于 Dart 核心库的更多内容，请参考
[Dart 核心库预览](/guides/libraries/library-tour)。

<div class="alert alert-info" markdown="1">
**注意：**
下面所介绍的大部分特性都可以在 
[DartPad](/tools/dartpad) 中运行。
</div>

如果你需要了解语言特性的更详细的细节，
请查看
[Dart 语言规范](/guides/language/spec) 。


## 一个最基本的 Dart 程序

下面的代码使用了很多 Dart 中最基本的特性：

<!-- language-tour/basic-dart-program/bin/main.dart -->
{% prettify dart %}
// 定义个方法。
printNumber(num aNumber) {
  print('The number is $aNumber.'); // 在控制台打印内容。
}

// 这是程序执行的入口。
main() {
  var number = 42; // 定义并初始化一个变量。
  printNumber(number); // 调用一个方法。
}
{% endprettify %}
<a href="{{ site.custom.dartpad.direct-link }}" class='run-in-dartpad' target="_blank">在 Dartpad 中运行</a>

下面是上面代码中使用的一些可以应用到几乎所有 Dart 应用中的
特性：

<code>// <em>这是一个注释。</em> </code>

:   使用 // 表示后面的文字都是注释。
    另外，你还可以使用 /\* ... \*/ 。详情请参考：
    [注释](#comments)。

`num`

:   一个类型。 String, int, 和 bool 是另外几种类型。

`42`

:   一个数字字面量。数字字面量是编译时常量。

`print()`

:   一种打印内容的助手方法。

`'...'` (或者 `"..."`)

:   字符串字面量。

<code>$<em>variableName</em></code> (or <code>${<em>expression</em>}</code>)

:   字符串插值：在字符串字面量中引用变量或者表达式。
    详情请参考：
    [Strings](#strings)。

`main()`

:   Dart 程序执行的入口方法，每个程序都 *需要* 一个这样的方法。
    详情请参考：
    [main() 方法](#the-main-function)。

`var`

:   一种不指定类型声明变量的方式。

<div class="alert alert-info" markdown="1">
**注意：**
我们的代码准守 
[Dart 代码样式](/guides/language/effective-dart/style)中的规则。
例如： 我们使用两个空格来缩进代码。
</div>


## 重要的概念

在学习 Dart 的时候，请牢记下面一些事实和
概念：

-   所有能够使用变量引用的都是*对象*，
    每个对象都是一个*类*的实例。在 Dart 中
    甚至连 数字、方法和 `null` 都是对象。所有的对象都继承于
    [Object]({{site.dart_api}}/dart-core/Object-class.html) 类。

-   使用静态类型(例如前面示例中的 `num` )
    可以更清晰的表明你的意图，并且可以让静态分析工具来分析你的代码，
    但这并不是牵制性的。（在调试代码的时候你可能注意到
    没有指定类型的变量的类型为 `dynamic`。）

-   Dart 在运行之前会先解析你的代码。你可以通过使用
    类型或者编译时常量来帮助 Dart 去捕获异常以及
    让代码运行的更高效。

-   Dart 支持顶级方法 (例如 `main()`)，同时还支持在类中定义函数。
    （静态函数和实例函数）。
    你还可以在方法中定义方法
    （嵌套方法或者局部方法）。

-   同样，Dart 还支持顶级变量，以及
    在类中定义变量（静态变量和实例变量）。
    实例变量有时候被称之为域（Fields）或者属性（Properties）。

-   和 Java 不同的是，Dart 没有 `public`、 `protected`、
    和 `private` 关键字。如果一个标识符以 (\_) 开头，则该标识符
    在库内是私有的。详情请参考：
    [库和可见性](#libraries-and-visibility)。

-   *标识符*可以以字母或者 \_ 下划线开头，后面可以是
    其他字符和数字的组合。

-   有时候 *表达式 expression* 和
    *语句 statement* 是有区别的，所以这种情况我们会分别指明每种情况。

-   Dart 工具可以指出两种问题：警告和错误。
    警告只是说你的代码可能有问题，
    但是并不会阻止你的代码执行。
    错误可以是编译时错误也可以是运行时错误。遇到编译时错误时，代码将
    无法执行；运行时错误将会在运行代码的时候导致一个
    [异常](#exceptions)。

<ul>
  <li id="runtime-modes" class="no_toc" markdown="1">
  Dart 有两种运行模式 <em id="runtime-modes">运行时模式</em>：
  生产模式和检查模式（checked mode）。我们建议你在
  检查模式开发，在生产模式部署代码。

  *生产模式* 是 Dart 程序运行的默认模式，
  优化了其性能。生产模式 忽略 [assert
  statements](#assert) 和静态类型检查。

  *检查模式* 是开发友好模式，可以帮助你检测一些运行时类型错误。
  例如，如果你把一个非数字变量赋值了一个类型为
  `num` 的变量，则在检查模式运行，将会抛出一个异常。
  </li>
</ul>

## 关键字

下表为 Dart 语言的关键字。

{% assign bii = '&nbsp;<sup title="built-in-identifier" alt="built-in-identifier">1</sup>' %}
{% assign lrw = '&nbsp;<sup title="limited reserved word" alt="limited reserved word">2</sup>' %}

| abstract{{bii}}   | continue          | false             | new               | this              |
| as{{bii}}         | default           | final             | null              | throw             |
| assert            | deferred{{bii}}   | finally           | operator{{bii}}   | true              |
| async{{lrw}}      | do                | for               | part{{bii}}       | try               |
| async*{{lrw}}     | dynamic{{bii}}    | get{{bii}}        | rethrow           | typedef{{bii}}    |
| await{{lrw}}      | else              | if                | return            | var               |
| break             | enum              | implements{{bii}} | set{{bii}}        | void              |
| case              | export{{bii}}     | import{{bii}}     | static{{bii}}     | while             |
| catch             | external{{bii}}   | in                | super             | with              |
| class             | extends           | is                | switch            | yield{{lrw}}      |
| const             | factory{{bii}}    | library{{bii}}    | sync*{{lrw}}      | yield*{{lrw}}     |
{:.table .table-striped .nowrap}

<sup>1</sup> 带有上标 **1** 的关键字是
*内置关键字*。避免把内置关键字当做标识符使用。
也不要把内置关键字
用作类名字和类型名字。
有些内置关键字是为了方便把 JavaScript 代码移植到 Dart 而存在的。
例如，如果 JavaScript 代码中有个变量的名字为 `factory`，
在移植到 Dart 中的时候，你不必重新命名这个变量。

<sup>2</sup> 带有上标 **2** 
的关键字，是在 Dart 1.0 发布以后又新加的，用于
支持异步相关的特性。
你不能在标记为
`async`、 `async*`、或者 `sync*` 的方法体内
使用 `async`、 `await`、或者 `yield` 作为标识符。
详情请参考：[异步支持](#asynchrony-support)。

所以其他单词都是 <em>保留词</em>。
你不能用保留词作为关键字。


## 变量

下面是声明变量并赋值的示例：

<!-- language-tour/creating-a-variable/bin/main.dart -->
{% prettify dart %}
var name = 'Bob';
{% endprettify %}

变量是一个引用。上面名字为 `name` 的变量引用了
一个内容为 “Bob” 的 String 对象。

### 默认值

没有初始化的变量自动获取一个默认值为 `null`。类型为数字的
变量如何没有初始化其值也是 null，不要忘记了 数字类型也是对象。

<!-- language-tour/numbers-are-objects/bin/main.dart -->
{% prettify dart %}
int lineCount;
assert(lineCount == null);
// Variables (even if they will be numbers) are initially null.
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
在生产模式 `assert()` 语句被忽略了。在检查模式
<code>assert(<em>condition</em>)</code>
会执行，如果条件不为 true 则会抛出一个异常。详情请参考
[Assert](#assert) 部分。
</div>


### 可选的类型

在声明变量的时候，你可以选择加上具体
类型：

<!-- language-tour/static-types/bin/main.dart -->
{% prettify dart %}
String name = 'Bob';
{% endprettify %}

添加类型可以更加清晰的表达你的意图。
IDE 编译器等工具有可以使用类型来更好的帮助你，
可以提供代码补全、提前发现 bug 等功能。

<div class="alert alert-info" markdown="1">
**注意：**
对于局部变量，这里准守
[代码风格推荐](/guides/language/effective-dart/design#type-annotations)
部分的建议，使用 `var` 而不是具体的类型来定义局部变量。
</div>


### Final 和 const

如果你以后不打算修改一个变量，使用 `final` 或者 `const`。
一个 final 变量只能赋值一次；一个 const 变量是编译时常量。
（Const 变量同时也是 final 变量。）
顶级的 final 变量或者类中的 final 变量在
第一次使用的时候初始化。

<div class="alert alert-info" markdown="1">
**注意：**
实例变量可以为 `final` 但是不能是 `const` 。
</div>

下面是 final 变量的示例：

<!-- language-tour/final-initialization/bin/main.dart -->
{% prettify dart %}
final name = 'Bob'; // Or: final String name = 'Bob';
// name = 'Alice';  // Uncommenting this causes an error
{% endprettify %}

`const` 变量为编译时常量。
如果 const 变量在类中，请定义为 `static const`。
可以直接定义 const 和其值，也
可以定义一个 const 变量使用其他 const 
变量的值来初始化其值。

<!-- language-tour/const/bin/main.dart -->
{% prettify dart %}
const bar = 1000000;       // Unit of pressure (dynes/cm2)
const atm = 1.01325 * bar; // Standard atmosphere
{% endprettify %}

`const` 关键字不仅仅只用来定义常量。
有可以用来创建不变的值，
还能定义构造函数为 const 类型的，这种类型
的构造函数创建的对象是不可改变的。任何变量都可以有一个不变的值。

<!-- language-tour/const-vs-const/bin/main.dart -->
{% prettify dart %}
// Note: [] creates an empty list.
// const [] creates an empty, immutable list (EIA).
var foo = const [];   // foo is currently an EIA.
final bar = const []; // bar will always be an EIA.
const baz = const []; // baz is a compile-time constant EIA.

// You can change the value of a non-final, non-const variable,
// even if it used to have a const value.
foo = [];

// You can't change the value of a final or const variable.
// bar = []; // Unhandled exception.
// baz = []; // Unhandled exception.
{% endprettify %}

关于使用 `const` 来创建不变的值的更多信息，请参考：
[Lists](#lists)、 [Maps](#maps)、 和 [Classes](#classes)。


## 内置的类型

Dart 内置支持下面这些类型：

- numbers
- strings
- booleans
- lists (也被称之为 *arrays*)
- maps
- runes (用于在字符串中表示 Unicode 字符)
- symbols

你可以直接使用字母量来初始化上面的这些类型。
例如 `'this is a string'` 是一个字符串字面量，
`true` 是一个布尔字面量。

由于 Dart 中每个变量引用的都是一个对象 -- 一个类的实例，
你通常使用构造函数来初始化变量。
一些内置的类型具有自己的构造函数。例如，
可以使用 `Map()`构造函数来创建一个 map，
就像这样 `new Map()`。


### 数字（Numbers）

Dart 支持两种类型的数字：

[`int`]({{site.dart_api}}/dart-core/int-class.html)

:   整数值，其取值通常位于
    -2<sup>53</sup> 和 2<sup>53</sup> 之间。

[`double`]({{site.dart_api}}/dart-core/double-class.html)

:   64-bit (双精度) 浮点数，符合
    IEEE 754 标准。

`int` 和 `double` 都是
[`num`]({{site.dart_api}}/dart-core/num-class.html) 的子类。
num 类型定义了基本的操作符，例如  +, -, /, 和 \*，
还定义了  `abs()`、` ceil()`、和 `floor()` 等
函数。
(位操作符，例如 \>\> 定义在 `int` 类中。)
如果 num 或者其子类型不满足你的要求，请参考
[dart:math]({{site.dart_api}}/dart-math/dart-math-library.html) 库。

<div class="alert alert-warning" markdown="1">
**注意：**
不在 
-2<sup>53</sup> 到 2<sup>53</sup> 范围内的整数在 Dart 中的行为
和 JavaScript 中表现不一样。
原因在于 Dart 具有任意精度的整数，而 JavaScript 没有。
参考 
[问题 1533](https://github.com/dart-lang/sdk/issues/1533) 了解更多信息。
</div>

整数是不带小数点的数字。下面是一些定义
整数的方式：

<!-- language-tour/integer-literals/bin/main.dart -->
{% prettify dart %}
var x = 1;
var hex = 0xDEADBEEF;
var bigInt = 34653465834652437659238476592374958739845729;
{% endprettify %}

如果一个数带小数点，则其为 double，
下面是定义 double 的一些方式：

<!-- language-tour/double-literals/bin/main.dart -->
{% prettify dart %}
var y = 1.1;
var exponents = 1.42e5;
{% endprettify %}

下面是字符串和数字之间转换的方式：

<!-- language-tour/number-conversion/bin/main.dart -->
{% prettify dart %}
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
{% endprettify %}

整数类型支持传统的位移操作符，(\<\<, \>\>), AND
(&), 和 OR (|) 。例如：

<!-- language-tour/bit-shifting/bin/main.dart -->
{% prettify dart %}
assert((3 << 1) == 6);  // 0011 << 1 == 0110
assert((3 >> 1) == 1);  // 0011 >> 1 == 0001
assert((3 | 4)  == 7);  // 0011 | 0100 == 0111
{% endprettify %}

数字字面量为编译时常量。
很多算术表达式
只要其操作数是常量，则表达式结果
也是编译时常量。

<!-- language-tour/number-literals/bin/main.dart -->
{% prettify dart %}
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
{% endprettify %}


### 字符串（Strings）

Dart 字符串是 UTF-16 编码的字符序列。
可以使用单引号或者双引号来创建字符串：

<!-- language-tour/quoting/bin/main.dart -->
{% prettify dart %}
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
{% endprettify %}

可以在字符串中使用表达式，用法是这样的：
`${`*`expression`*`}`。如果表达式是一个比赛服，可以省略
{}。 如果表达式的结果为一个对象，则 Dart 会调用对象的
 `toString()` 函数来获取一个字符串。

<!-- language-tour/string-interpolation/bin/main.dart -->
{% prettify dart %}
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' ==
       'Dart has string interpolation, ' +
       'which is very handy.');
assert('That deserves all caps. ' +
       '${s.toUpperCase()} is very handy!' ==
       'That deserves all caps. ' +
       'STRING INTERPOLATION is very handy!');
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
`==` 操作符判断两个对象的内容是否一样。
如果两个字符串包含一样的字符编码序列，
则他们是相等的。
</div>

可以使用 `+` 操作符来把多个字符串链接为一个，也可以把多个
字符串放到一起来实现同样的功能：

<!-- adjacent_string_literals.dart -->
{% prettify dart %}
var s1 = 'String ' 'concatenation'
         " works even over line breaks.";
assert(s1 == 'String concatenation works even over '
             'line breaks.');

var s2 = 'The + operator '
         + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
{% endprettify %}

使用三个单引号或者双引号也可以
创建多行字符串对象：

<!-- language-tour/triple-quotes/bin/main.dart -->
{% prettify dart %}
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
{% endprettify %}

通过提供一个  `r` 前缀可以创建一个 “原始 raw” 字符串：

<!-- language-tour/raw-strings/bin/main.dart -->
{% prettify dart %}
var s = r"In a raw string, even \n isn't special.";
{% endprettify %}

参考 [Runes](#runes) 来了解如何在字符串
中表达 Unicode 字符。

字符串字面量是编译时常量，
带有字符串插值的字符串定义，若干插值表达式引用的为编译时常量则其结果也是编译时常量。

<!-- language-tour/string-literals/bin/main.dart -->
{% prettify dart %}
// These work in a const string.
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'a constant string';

// These do NOT work in a const string.
var aNum = 0;
var aBool = true;
var aString = 'a string';
const aConstList = const [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
{% endprettify %}

使用字符串的更多信息请参考：
[字符串和正则表达式](/guides/libraries/library-tour#strings-and-regular-expressions)。


### 布尔值（Booleans）

为了代表布尔值，Dart 有一个名字为 `bool` 的类型。
只有两个对象是布尔类型的：`true` 和 `false` 所创建的对象，
这两个对象也都是编译时常量。

当 Dart 需要一个布尔值的时候，只有 `true`  对象才被认为是 true。
所有其他的值都是 flase。这点和 JavaScript 不一样，
像 `1`、 `"aString"`、 以及 `someObject` 等值都被认为是 
false。

例如，下面的代码在
JavaScript 和 Dart 中都是合法的代码：

<!-- language-tour/reference/strictly_booleans.dart -->
{% prettify dart %}
var name = 'Bob';
if (name) {
  // Prints in JavaScript, not in Dart.
  print('You have a name!');
}
{% endprettify %}

如果在 JavaScript 中运行，则会打印出 “You have a name!”，在 JavaScript 中
`name` 是非 null 对象所以认为是 true。但是在 Dart 的*生产模式*下
运行，这不会打印任何内容，原因是 `name` 被转换为 false了，原因在于
`name != true`。
如果在 Dart *检查模式*运行，上面的
代码将会抛出一个异常，表示 `name` 变量不是一个布尔值。

{% comment %}
xxx: This code also produces an error:
     "Conditions must have a static type of "bool".
{% endcomment %}

下面是另外一个在 JavaScript
和 Dart 中表现不一致的示例：

<!-- language-tour/if-one/bin/main.dart -->
{% prettify dart %}
if (1) {
  print('JS prints this line.');
} else {
  print('Dart in production mode prints this line.');
  // However, in checked mode, if (1) throws an
  // exception because 1 is not boolean.
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
上面两个示例只能在 Dart 生产模式下运行，
在检查模式下，会抛出异常表明
变量不是所期望的布尔类型。
</div>

{% comment %}
xxx: The previous example also produces the same error as above.
{% endcomment %}

Dart 这样设计布尔值，是为了避免奇怪的行为。很多 JavaScript 代码
都遇到这种问题。
对于你来说，在写代码的时候你不用这些写代码：
<code>if (<em>nonbooleanValue</em>)</code>，你应该显式的
判断变量是否为布尔值类型。例如：

<!-- language-tour/empty-string/bin/main.dart -->
{% prettify dart %}
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints <= 0);

// Check for null.
var unicorn;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
{% endprettify %}


### 列表（Lists）

也许 *array* （或者有序集合）是所有编程语言中最常见的集合类型。
在 Dart 中数组就是
[List]({{site.dart_api}}/dart-core/List-class.html) 对象。所以
通常我们都称之为 *lists*。

Dart list 字面量和 JavaScript 的数组字面量类似。下面是一个
Dart list 的示例：

<!-- language-tour/list-literal/bin/main.dart -->
{% prettify dart %}
var list = [1, 2, 3];
{% endprettify %}

Lists 的下标索引从 0 开始，第一个元素的索引是 0.
`list.length - 1` 是最后一个元素的索引。
访问 list 的长度和元素与 
JavaScript 中的用法一样：

<!-- language-tour/list-indexing/bin/main.dart -->
{% prettify dart %}
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
{% endprettify %}

在 list 字面量之前添加 `const` 关键字，可以
定义一个不变的 list 对象（编译时常量）：

<!-- language-tour/list-literal/bin/main.dart -->
{% prettify dart %}
var constantList = const [1, 2, 3];
// constantList[1] = 1; // Uncommenting this causes an error.
{% endprettify %}

List 类型有很多函数可以操作 list。
更多信息参考 [泛型](#generics) 和
[集合](/guides/libraries/library-tour#collections)。


### Maps

通常来说，Map 是一个键值对相关的对象。
键和值可以是任何类型的对象。每个 *键* 只出现一次，
而一个值则可以出现多次。Dart 通过 map 字面量
和
[Map]({{site.dart_api}}/dart-core/Map-class.html) 类型支持 map。

下面是一些创建简单 map 的示例：

<!-- language-tour/reference/map_literal.dart -->
{% prettify dart %}
var gifts = {
// Keys      Values
  'first' : 'partridge',
  'second': 'turtledoves',
  'fifth' : 'golden rings'
};

var nobleGases = {
// Keys  Values
  2 :   'helium',
  10:   'neon',
  18:   'argon',
};
{% endprettify %}

使用 Map 构造函数也可以实现同样的功能：

<!-- language-tour/map-constructor/bin/main.dart -->
{% prettify dart %}
var gifts = new Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = new Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
{% endprettify %}

往 map 中添加新的键值对和在
JavaScript 中的用法一样：

<!-- language-tour/map-add-item/bin/main.dart -->
{% prettify dart %}
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // Add a key-value pair
{% endprettify %}

获取 map 中的对象也和 JavaScript 的用法一样：

<!-- language-tour/map-retrieve-item/bin/main.dart -->
{% prettify dart %}
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
{% endprettify %}

如果所查找的键不存在，则返回 null：

<!-- language-tour/map-missing-key/bin/main.dart -->
{% prettify dart %}
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
{% endprettify %}

使用 `.length` 来获取 map 中键值对的数目：

<!-- language-tour/map-length/bin/main.dart -->
{% prettify dart %}
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
{% endprettify %}

同样使用 `const` 可以创建一个
编译时常量的 map：

<!-- language-tour/reference/map_literal.dart -->
{% prettify dart %}
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // Uncommenting this causes an error.
{% endprettify %}

关于 Map 的更多信息请参考
[泛型](#generics) 和
[Maps](/guides/libraries/library-tour#maps)。

### Runes

在 Dart 中，runes 代表字符串的 UTF-32 code points。

Unicode 为每一个字符、标点符号、表情符号等都定义了
一个唯一的数值。
由于 Dart 字符串是 UTF-16 code units 字符序列，
所以在字符串中表达 32-bit Unicode 值就需要
新的语法了。

通常使用 `\uXXXX` 的方式来表示 Unicode code point，
这里的 XXXX 是4个 16 进制的数。
例如，心形符号 (♥) 是 `\u2665`。
对于非 4 个数值的情况，
把编码值放到大括号中即可。
例如，笑脸 emoji (😆) 是 `\u{1f600}`。

[String]({{site.dart_api}}/dart-core/String-class.html) 类
有一些属性可以提取 rune 信息。
`codeUnitAt` 和 `codeUnit` 属性返回 16-bit code units。
使用 `runes` 属性来获取字符串的 runes 信息。

下面是示例演示了 runes、
16-bit code units、和 32-bit code points 之间的关系。
点击运行按钮 ( {% img 'red-run.png' %} )
 查看 runes 。

<!-- language-tour/runes/bin/main.dart -->
{% comment %}
https://gist.github.com/589bc5c95318696cefe5
https://dartpad.dartlang.org/589bc5c95318696cefe5
Unicode emoji: http://unicode.org/emoji/charts/full-emoji-list.html

main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(new String.fromCharCodes(input));
}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-dart-prefix}}?id=589bc5c95318696cefe5&horizontalRatio=99&verticalRatio=65"
    width="100%"
    height="310px"
    style="border: 1px solid #ccc;">
</iframe>

<div class="alert alert-warning" markdown="1">
**注意：**
使用 list 操作 runes 的时候请小心。
根据所操作的语种、字符集等，
这种操作方式可能导致你的字符串出问题。
更多信息参考 Stack Overflow 上的一个问题：
[我如何在 Dart 中反转一个字符串？](http://stackoverflow.com/questions/21521729/how-do-i-reverse-a-string-in-dart)
</div>

### Symbols

一个 [Symbol]({{site.dart_api}}/dart-core/Symbol-class.html) object
代表 Dart 程序中声明的操作符或者标识符。
你也许从来不会用到 Symbol，但是该功能对于通过名字来引用标识符的情况
是非常有价值的，特别是混淆后的代码，
标识符的名字被混淆了，但是 Symbol 的名字不会改变。

使用 Symbol 字面量来获取标识符的 symbol 对象，也就是在标识符
前面添加一个 `#` 符号：

<!-- language-tour/symbols/bin/main.dart -->
{% prettify dart %}
#radix
#bar
{% endprettify %}

Symbol 字面量定义是编译时常量。

关于 symbols 的详情，请参考
[dart:mirrors - reflection](/guides/libraries/library-tour#dartmirrors---reflection)。


## 方法（Functions）

Dart 是一个真正的面向对象语言，方法也是对象并且具有一种
类型，
[`Function`]({{site.dart_api}}/dart-core/Function-class.html)。
这意味着，方法可以赋值给变量，也可以当做其他方法的参数。
也可以把 Dart 类的实例当做方法来调用。
详情请参考 [Callable classes](#callable-classes)。

下面是定义方法的示例：

<!-- language-tour/function-example/bin/main.dart -->
{% prettify dart %}
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
{% endprettify %}

虽然在 Effective Dart 中推荐
[在公开的 APIs 上使用静态类型](/guides/language/effective-dart/design#do-type-annotate-public-apis)，
你当然也可以选择忽略类型定义：

<!-- language-tour/function-omitting-types/bin/main.dart -->
{% prettify dart %}
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
{% endprettify %}

对于只有一个表达式的方法，你可以选择
使用缩写语法来定义：

<!-- language-tour/function-shorthand/bin/main.dart -->
{% prettify dart %}
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
{% endprettify %}

这个 <code>=> <em>expr</em></code> 语法是
<code>{ return <em>expr</em>; }</code> 形式的缩写。`=>` 形式
有时候也称之为 _胖箭头_ 语法。

<div class="alert alert-info" markdown="1">
**注意：**
在箭头 (=\>) 和冒号 (;) 之间只能使用一个 *表达式* -- 不能使用 *语句*。
例如：你不能使用 [if statement](#if-and-else)，但是可以
使用条件表达式
[conditional expression](#conditional-expressions)。
</div>

方法可以有两种类型的参数：必需的和可选的。
必需的参数在参数列表前面，
后面是可选参数。


### 可选参数 （Optional parameters）

可选参数可以是命名参数或者基于位置的参数，但是这两种参数不能同时当做可选参数。

#### 可选命名参数 （Optional named parameters）

调用方法的时候，你可以使用这种形式 
<code><em>paramName</em>: <em>value</em></code> 来指定命名参数。例如：

<!-- language-tour/use-named-parameters/bin/main.dart -->
{% prettify dart %}
enableFlags(bold: true, hidden: false);
{% endprettify %}

在定义方法的时候，使用 
<code>{<em>param1</em>, <em>param2</em>, …}</code>
的形式来指定命名参数：

<!-- language-tour/specify-named-parameters/bin/main.dart -->
{% prettify dart %}
/// Sets the [bold] and [hidden] flags to the values
/// you specify.
enableFlags({bool bold, bool hidden}) {
  // ...
}
{% endprettify %}

#### 可选位置参数 （Optional positional parameters）

把一些方法的参数放到 `[]` 中就变成可选
位置参数了：

<!-- language-tour/optional-positional-parameters/bin/main.dart -->
{% prettify dart %}
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
{% endprettify %}

下面是不使用可选参数调用上面方法
的示例：

<!-- language-tour/optional-positional-parameters/bin/main.dart -->
{% prettify dart %}
assert(say('Bob', 'Howdy') == 'Bob says Howdy');
{% endprettify %}

下面是使用可选参数调用上面方法的示例：

<!-- language-tour/optional-positional-parameters/bin/main.dart -->
{% prettify dart %}
assert(say('Bob', 'Howdy', 'smoke signal') ==
    'Bob says Howdy with a smoke signal');
{% endprettify %}

<a id="default-parameters"></a>
#### 带默认值的参数 （Default parameter values）

在定义方法的时候，可以使用 `=` 来定义可选参数的默认值。
默认值只能是编译时常量。
如果没有提供默认值，则默认值为 `null`。

下面是设置可选参数默认值的示例：

<!-- language-tour/specify-default-values/bin/main.dart -->
{% prettify dart %}
/// Sets the [bold] and [hidden] flags to the values you
/// specify, defaulting to false.
void enableFlags({bool bold = false, bool hidden = false}) {
  // ...
}

// bold will be true; hidden will be false.
enableFlags(bold: true);
{% endprettify %}

<div class="alert alert-info" markdown="1">
**版本问题：**
就版本代码可能需要使用一个冒号 (`:`) 而不是 `=`
来设置参数默认值。
原因在于 Dart SDK 1.21 之前的版本，命名参数只支持 `:`。
`:` 设置命名默认参数值在以后版本中将不能使用，
所以我们推荐你
**使用 `=` 来设置默认值，
并 [指定 Dart SDK 版本为 1.21 或者更高的版本。](/tools/pub/pubspec#sdk-constraints)**
</div>

下面的示例显示了如何设置位置参数的默认值：

<!-- language-tour/optional-positional-parameter-default/bin/main.dart -->
{% prettify dart %}
String say(String from, String msg,
    [String device = 'carrier pigeon', String mood]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  if (mood != null) {
    result = '$result (in a $mood mood)';
  }
  return result;
}

assert(say('Bob', 'Howdy') ==
    'Bob says Howdy with a carrier pigeon');
{% endprettify %}

还可以使用 list 或者 map 作为默认值。
下面的示例定义了一个方法 `doStuff()`，
并分别为 `list` 和 `gifts` 参数指定了
默认值。
{% comment %}
The function is called three times with different values.
Click the run button ( {% img 'red-run.png' %} )
to see list and map default values in action.
{% endcomment %}

<!-- language-tour/list-map-default-function-parameters/bin/main.dart -->
{% prettify dart %}
void doStuff(
    {List<int> list = const [1, 2, 3],
    Map<String, String> gifts = const {
      'first': 'paper',
      'second': 'cotton',
      'third': 'leather'
    }}) {
  print('list:  $list');
  print('gifts: $gifts');
}
{% endprettify %}

{% comment %}
https://gist.github.com/d988cfce0a54c6853799
https://dartpad.dartlang.org/d988cfce0a54c6853799
(The gist needs updating: see https://github.com/dart-lang/site-www/issues/189)
<iframe
src="{{site.custom.dartpad.embed-dart-prefix}}?id=d988cfce0a54c6853799&horizontalRatio=99&verticalRatio=70"
    width="100%"
    height="450px"
    style="border: 1px solid #ccc;">
</iframe>
{% endcomment %}


### 入口函数 （The main() function）

每个应用都需要有个顶级的 `main()`  入口方法才能执行。
`main()` 方法的返回值为 `void` 并且有个可选的
 `List<String>` 参数。

下面是一个 web 应用的 `main()` 方法：

<!-- from Dart Editor's default web app -->
{% prettify dart %}
void main() {
  querySelector("#sample_text_id")
    ..text = "Click me!"
    ..onClick.listen(reverseText);
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
前面代码中的 `..` 语法为 [级联调用](#cascade-notation-)（cascade）。
使用级联调用语法，
你可以在一个对象上执行多个操作。
</div>

下面是一个命令行应用的 `main()` 方法，并且使用了
方法参数作为输入参数：

<!-- language-tour/args/bin/main.dart -->
{% prettify dart %}
// Run the app like this: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
{% endprettify %}

你可以使用 [args library](https://pub.dartlang.org/packages/args) 来
定义和解析命令行输入的参数数据。

### 一等方法对象 （Functions as first-class objects）

可以把方法当做参数调用另外一个方法。例如：

<!-- from language-tour/function-as-parameter/bin/main.dart -->
{% prettify dart %}
printElement(element) {
  print(element);
}

var list = [1, 2, 3];

// Pass printElement as a parameter.
list.forEach(printElement);
{% endprettify %}

方法也可以赋值给一个变量：

<!-- from language-tour/function-as-variable/bin/main.dart -->
{% prettify dart %}
var loudify = (msg) => '!!! ${msg.toUpperCase()} !!!';
assert(loudify('hello') == '!!! HELLO !!!');
{% endprettify %}

上面的方法为
下面即将介绍的匿名方法。

### 匿名方法 （Anonymous functions）

大部分方法都带有名字，例如 `main()` 或者 `printElement()`。
你有可以创建没有名字的方法，称之为
_匿名方法_，有时候也被称为 _lambda_ 或者 _closure 闭包_。
你可以把匿名方法赋值给一个变量，
然后你可以使用这个方法，比如添加到集合或者从集合中删除。

匿名函数和命名函数看起来类似&mdash;
在括号之间可以定义一些参数，参数使用逗号
分割，也可以是可选参数。
后面大括号中的代码为函数体：

<code>
([[<em>Type</em>] <em>param1</em>[, …]]) { <br>
&nbsp;&nbsp;<em>codeBlock</em>; <br>
}; <br>
</code>

下面的代码定义了一个参数为`i` （该参数没有指定类型）的匿名函数。
list 中的每个元素都会调用这个函数来
打印出来，同时来计算了每个元素在 list 中的索引位置。

{% prettify dart %}
var list = ['apples', 'oranges', 'grapes', 'bananas', 'plums'];
list.forEach((i) {
  print(list.indexOf(i).toString() + ': ' + i);
});
{% endprettify %}

点击运行按钮 ( {% img 'red-run.png' %} ) 执行下面的代码。

{% comment %}
https://gist.github.com/Sfshaza/d1c5d3124c8abf2d58f9b98936339232
https://dartpad.dartlang.org/d1c5d3124c8abf2d58f9b98936339232

main() {
  var list = ['apples', 'oranges', 'grapes', 'bananas', 'plums'];
  list.forEach((i) {
    print(list.indexOf(i).toString() + ': ' + i);
  });
}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-dart-prefix}}?id=d1c5d3124c8abf2d58f9b98936339232&horizontalRatio=99&verticalRatio=50"
    width="100%"
    height="250px"
    style="border: 1px solid #ccc;">
</iframe>

如果方法只包含一个语句，可以使用胖箭头语法缩写。
把下面的代码粘贴到
DartPad 中运行，可以看到结果是一样的。

{% prettify dart %}
list.forEach((i) => print(list.indexOf(i).toString() + ': ' + i));
{% endprettify %}


### 静态作用域 （Lexical scope）

Dart 是静态作用域语言，变量的作用域在写代码的时候就确定过了。
基本上大括号里面定义的变量就
只能在大括号里面访问，和 Java 作用域
类似。

下面是作用域的一个
示例：

<!-- language-tour/nested-functions/bin/main.dart -->
{% prettify dart %}
var topLevel = true;

main() {
  var insideMain = true;

  myFunction() {
    var insideFunction = true;

    nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
{% endprettify %}

注意 `nestedFunction()` 可以访问所有的变量，
包含顶级变量。


### 词法闭包 （Lexical closures）

一个 *闭包* 是一个方法对象，不管该对象在何处被调用，
该对象都可以访问其作用域内
的变量。

方法可以封闭定义到其作用域内的变量。
下面的示例中，`makeAdder()` 捕获到了变量 `addBy`。
不管你在那里执行 `makeAdder()` 所返回的函数，都可以使用 `addBy` 参数。

<!-- language-tour/function-closure/bin/main.dart -->
{% prettify dart %}
/// Returns a function that adds [addBy] to the
/// function's argument.
Function makeAdder(num addBy) {
  return (num i) => addBy + i;
}

main() {
  // Create a function that adds 2.
  var add2 = makeAdder(2);

  // Create a function that adds 4.
  var add4 = makeAdder(4);

  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
{% endprettify %}


### 测试函数是否相等（Testing functions for equality）

下面是测试顶级方法、静态函数和实例函数
相等的示例：

<!-- language-tour/function-equality-2/bin/main.dart -->
{% prettify dart %}
foo() {}               // A top-level function

class A {
  static void bar() {} // A static method
  void baz() {}        // An instance method
}

main() {
  var x;

  // Comparing top-level functions.
  x = foo;
  assert(foo == x);

  // Comparing static methods.
  x = A.bar;
  assert(A.bar == x);

  // Comparing instance methods.
  var v = new A(); // Instance #1 of A
  var w = new A(); // Instance #2 of A
  var y = w;
  x = w.baz;

  // These closures refer to the same instance (#2),
  // so they're equal.
  assert(y.baz == x);

  // These closures refer to different instances,
  // so they're unequal.
  assert(v.baz != w.baz);
}
{% endprettify %}


### 返回值 （Return values）

所有的函数都返回一个值。如果没有指定返回值，则
默认把语句 `return null;` 作为函数的最后一个语句执行。


## 操作符 （Operators）

下表是 Dart 中定义的操作符。 
很多操作符都可以重载，详情参考
[Overridable operators](#overridable-operators)。

|--------------------------+------------------------------------------------|
|描述                      | 操作符                                         |
|--------------------------|------------------------------------------------|
| unary postfix            | <code><em>expr</em>++</code>    <code><em>expr</em>--</code>    `()`    `[]`    `.`    `?.` |
| unary prefix             | <code>-<em>expr</em></code>    <code>!<em>expr</em></code>    <code>~<em>expr</em></code>    <code>++<em>expr</em></code>    <code>--<em>expr</em></code>   |
| multiplicative           | `*`    `/`    `%`    `~/`                      |
| additive                 | `+`    `-`                                     |
| shift                    | `<<`    `>>`                                   |
| bitwise AND              | `&`                                            |
| bitwise XOR              | `^`                                            |
| bitwise OR               | `|`                                            |
| relational&nbsp;and&nbsp;type&nbsp;test | `>=`    `>`    `<=`    `<`    `as`    `is`    `is!` |
| equality                 | `==`    `!=`                                   |
| logical AND              | `&&`                                           |
| logical OR               | `||`                                           |
| if null                  | `??`                                           |
| conditional              | <code><em>expr1</em> ? <em>expr2</em> : <em>expr3</em></code> |
| cascade                  | `..`                                           |
| assignment               | `=`    `*=`    `/=`    `~/=`    `%=`    `+=`    `-=`    `<<=`    `>>=`    `&=`    `^=`    `|=`    `??=` |
{:.table .table-striped}

在使用操作符的时候，就创建了表达式。下面是一些
操作符表达式：

<!-- TODO: write test for this -->
{% prettify dart %}
a++
a + b
a = b
a == b
a ? b: c
a is T
{% endprettify %}

在[操作符表格](#operators)中所列的操作符
都是按照优先级顺序从左到右，从上到下的方式来列出的，
上面和左边的操作符优先级要高于下面和右边的。
例如 `%`  操作符优先级高于 `==`，而 等号
高于 `&&`。所以下面的
代码结果是一样的：

<!-- language-tour/precedence/bin/main.dart -->
{% prettify dart %}
// 1: Parens improve readability.
if ((n % i == 0) && (d % i == 0))

// 2: Harder to read, but equivalent.
if (n % i == 0 && d % i == 0)
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**警告：**
对于有两个操作数的操作符，左边的操作数决定了
操作符的功能。
例如，如果有一个 Vector 对象和一个 Point 对象， `aVector + aPoint`
使用的是 Vector 对象中定义的 + 操作符。
</div>


### 算术操作符 （Arithmetic operators）

Dart 支持常用的算术操作符，如下：

|-----------------------------+-------------------------------------------|
| 操作符                      | 解释                                      |
|-----------------------------+-------------------------------------------|
| `+`                         | 加号
| `–`                         | 减号
| <code>-<em>expr</em></code> | 负号
| `*`                         | 乘号
| `/`                         | 除号
| `~/`                        | 除号，但是返回值为整数
| `%`                         | 取模
{:.table .table-striped}

示例：

<!-- language-tour/arithmetic-operators/bin/main.dart -->
{% prettify dart %}
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5);   // Result is a double
assert(5 ~/ 2 == 2);    // Result is an integer
assert(5 % 2 == 1);     // Remainder

print('5/2 = ${5~/2} r ${5%2}'); // 5/2 = 2 r 1
{% endprettify %}

Dart 还支持递增、递减前缀
和后缀操作：

|-----------------------------+-------------------------------------------|
| Operator                    | Meaning                                   |
|-----------------------------+-------------------------------------------|
| <code>++<em>var</em></code> | <code><em>var</em> = <em>var</em> + 1</code> (expression value is <code><em>var</em> + 1</code>)
| <code><em>var</em>++</code> | <code><em>var</em> = <em>var</em> + 1</code> (expression value is <code><em>var</em></code>)
| <code>--<em>var</em></code> | <code><em>var</em> = <em>var</em> – 1</code> (expression value is <code><em>var</em> – 1</code>)
| <code><em>var</em>--</code> | <code><em>var</em> = <em>var</em> – 1</code> (expression value is <code><em>var</em></code>)
{:.table .table-striped}

示例：

<!-- language-tour/op-increment-decrement/bin/main.dart -->
{% prettify dart %}
var a, b;

a = 0;
b = ++a;        // Increment a before b gets its value.
assert(a == b); // 1 == 1

a = 0;
b = a++;        // Increment a AFTER b gets its value.
assert(a != b); // 1 != 0

a = 0;
b = --a;        // Decrement a before b gets its value.
assert(a == b); // -1 == -1

a = 0;
b = a--;        // Decrement a AFTER b gets its value.
assert(a != b); // -1 != 0
{% endprettify %}


### 相等相关的操作符 （Equality and relational operators）

下表是和相等操作相关的操作符。

|-----------+-------------------------------------------|
| 操作符    | 解释                                      |
|-----------+-------------------------------------------|
| `==`      |       相等
| `!=`      |       不等
| `>`       |       大于
| `<`       |       小于
| `>=`      |       大于等于
| `<=`      |       小于等于
{:.table .table-striped}


要测试两个对象代表的是否为同样的内容，使用
`==` 操作符。(在某些情况下，你需要知道两个对象是否是同一个对象，
使用 
[`identical()`]({{site.dart_api}}/dart-core/identical.html)
 方法。) 下面是 `==` 操作符工作原理解释：

1.  如果 *x* 或者 *y* 是 null，如果两个都是 null 则返回 true，如果
    只有一个是 null 返回 false。

2.  返回如下函数的返回值 
    <code><em>x</em>.==(<em>y</em>)</code>。 （你没看错，
    像 `==` 这种操作符是定义在左边对象上的函数。
    你甚至还可以覆写这些操作符。
    在后面的
    [Overridable operators](#overridable-operators) 将介绍如何做。）

下面是相等关系操作符的
使用示例：

<!-- language-tour/op-equality/bin/main.dart -->
{% prettify dart %}
assert(2 == 2);
assert(2 != 3);
assert(3 > 2);
assert(2 < 3);
assert(3 >= 3);
assert(2 <= 3);
{% endprettify %}


### 类型判定操作符 （Type test operators）

`as`、 `is`、 和 `is!` 操作符是在运行时判定对象
类型的操作符：

|-----------+-------------------------------------------|
| 操作符    | 解释                                   |
|-----------+-------------------------------------------|
| `as`      | 类型转换
| `is`      | 如果对象是指定的类型返回 True
| `is!`     | 如果对象是指定的类型返回 False
{:.table .table-striped}

只有当 `obj` 实现了 `T` 的接口，
`obj is T` 才是 true。例如 `obj is Object` 总是 true。

使用 `as` 操作符把对象转换为特定的类型。
一般情况下，你可以把它当做用 `is` 判定类型然后调用
所判定对象的函数的缩写形式。例如下面的
示例：

<!-- language-tour/op-as/bin/main.dart -->
{% prettify dart %}
if (emp is Person) { // Type check
  emp.firstName = 'Bob';
}
{% endprettify %}

使用 `as` 操作符可以简化上面的代码：

<!-- language-tour/op-as/bin/main.dart.dart -->
{% prettify dart %}
(emp as Person).firstName = 'Bob';
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
上面这两个代码效果是有区别的。如果 `emp` 是 null 或者不是 Person 类型，
则第一个示例使用 `is` 则不会执行条件里面的代码，而第二个情况使用
`as` 则会抛出一个异常。
</div>


### 赋值操作符 （Assignment operators）

使用 `=` 操作符来赋值。
但是还有一个 `??=` 操作符用来指定
值为 null 的变量的值。

{% prettify dart %}
a = value;   // 给 a 变量赋值
b ??= value; // 如果 b 是 null，则赋值给 b；
             // 如果不是 null，则 b 的值保持不变
{% endprettify %}

{% comment %}
<!-- embed a dartpad when we can hide code -->
https://gist.github.com/9de887c4daf76d39e524
https://dartpad.dartlang.org/9de887c4daf76d39e524
{% endcomment %}

还有复合赋值操作符 `+=` 等可以
赋值：

| `=`  | `–=` | `/=`  | `%=`  | `>>=` | `^=`
| `+=` | `*=` | `~/=` | `<<=` | `&=`  | `|=`
{:.table}

下面是复合赋值操作符工作原理解释：

|-----------+----------------------+-----------------------|
|           | 复合赋值操作符       | 相等的表达式          |
|-----------+----------------------+-----------------------|
|**对于  操作符 <em>op</em>:** | <code>a <em>op</em>= b</code> | <code>a = a <em>op</em> b</code>
|**示例:**                        |`a += b`                       | `a = a + b`
{:.table}

下面的代码使用赋值操作符合
复合赋值操作符：

<!-- language-tour/op-assign/bin/main.dart -->
{% prettify dart %}
var a = 2;           // Assign using =
a *= 3;              // Assign and multiply: a = a * 3
assert(a == 6);
{% endprettify %}


### 逻辑操作符 （Logical operators）

可以使用逻辑操作符来
操作布尔值：

|-----------------------------+-------------------------------------------|
| 操作符                      | 解释                                      |
|-----------------------------+-------------------------------------------|
| <code>!<em>expr</em></code> | 对表达式结果取反（true 变为 false ，false 变为 true）
| `||`                        | 逻辑 OR
| `&&`                        | 逻辑 AND
{:.table .table-striped}

下面是使用示例：

<!-- language-tour/op-logicalbin/main.dart -->
{% prettify dart %}
if (!done && (col == 0 || col == 3)) {
  // ...Do something...
}
{% endprettify %}


### 位于和移位操作符（Bitwise and shift operators）

在 Dart 中可以单独操作数字的某一位，
下面操作符同样应用于整数：

|-----------------------------+-------------------------------------------|
| 操作符                      | 解释                                      |
|-----------------------------+-------------------------------------------|
| `&`                         | AND
| `|`                         | OR
| `^`                         | XOR
| <code>~<em>expr</em></code> | Unary bitwise complement (0s become 1s; 1s become 0s)
| `<<`                        | Shift left
| `>>`                        | Shift right
{:.table .table-striped}

使用位于和移位操作符的示例：

<!-- language-tour/op-bitwise/bin/main.dart -->
{% prettify dart %}
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask)  == 0x02);  // AND
assert((value & ~bitmask) == 0x20);  // AND NOT
assert((value | bitmask)  == 0x2f);  // OR
assert((value ^ bitmask)  == 0x2d);  // XOR
assert((value << 4)       == 0x220); // Shift left
assert((value >> 4)       == 0x02);  // Shift right
{% endprettify %}


### 条件表达式（Conditional expressions）

Dart 有两个特殊的操作符可以用来替代
[if-else](#if-and-else) 语句：

<code><em>condition</em> ? <em>expr1</em> : <em>expr2</em>
: 如果 _condition_ 是 true，执行 _expr1_ (并返回执行的结果)；
  否则执行 _expr2_ 并返回其结果。

<code><em>expr1</em> ?? <em>expr2</em></code>
: 如果 _expr1_ 是 non-null，返回其值；
  否则执行 _expr2_ 并返回其结果。

如果你需要基于布尔表达式
的值来赋值，
考虑使用 `?:`。

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
var finalStatus = m.isFinal ? 'final' : 'not final';
{% endprettify %}

如果布尔表达式是测试值是否为 null，
考虑使用 `??`。

{% prettify dart %}
String toString() => msg ?? super.toString();
{% endprettify %}

下面是一样效果的实现，
但是代码不是那么简洁：

{% comment %}
https://dartpad.dartlang.org/f2c8d82ce5d0dd533fe2
https://gist.github.com/f2c8d82ce5d0dd533fe2
{% endcomment %}

{% prettify dart %}
// Slightly longer version uses ?: operator.
String toString() => msg == null ? super.toString() : msg;

// Very long version uses if-else statement.
String toString() {
  if (msg == null) {
    return super.toString();
  } else {
    return msg;
  }
}
{% endprettify %}

<a id="cascade"></a>
### 级联操作符（Cascade notation (..)）

级联操作符 (`..`) 可以在同一个对象上
连续调用多个函数以及访问成员变量。
使用级联操作符可以避免创建
临时变量，
并且写出来的代码看起来
更加流畅：

例如下面的代码：

<!-- language-tour/cascade-operator/web/main.dart -->
{% prettify dart %}
querySelector('#button') // Get an object.
  ..text = 'Confirm'   // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
{% endprettify %}

第一个方法 `querySelector()` 返回了一个 selector 对象。
后面的级联操作符都是调用这个对象的成员，
并忽略每个操作
所返回的值。

上面的代码和下面的代码功能一样：

{% prettify dart %}
var button = querySelector('#button');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
{% endprettify %}

级联调用也可以嵌套：

{% prettify dart %}
final addressBook = (new AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (new PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
{% endprettify %}

在方法上使用级联操作符需要非常小心，
例如下面的代码就是不合法的：

{% prettify dart %}
// Does not work
var sb = new StringBuffer();
sb.write('foo')..write('bar');
{% endprettify %}

`sb.write()` 函数返回一个 void，
无法在 `void` 上使用级联操作符。

<div class="alert alert-info" markdown="1">
**注意：**
严格来说，
两个点的级联语法不是一个操作符。
只是一个 Dart 特殊语法。
</div>

### 其他操作符 （Other operators）

下面是其他操作符：

|----------+-------------------------------------------|
| Operator | Name                 |          Meaning   |
|-----------+------------------------------------------|
| `()`     | 使用方法             | 代表调用一个方法
| `[]`     | 访问 List            | 访问 list 中特定位置的元素
| `.`      | 访问 Member          | 访问元素，例如 `foo.bar` 代表访问 `foo` 的 `bar` 成员
| `?.`     | 条件成员访问         | 和 `.` 类似，但是左边的操作对象不能为 null，例如 `foo?.bar` 如果 `foo` 为 null 则返回 null，否则返回 `bar` 成员
{:.table .table-striped}

关于 `.`、 `?.`、和 `..` 的详情，请参考
[Classes](#classes)。


## 流程控制语句（Control flow statements）

You can control the flow of your Dart code using any of the following:

-   `if` and `else`

-   `for` loops

-   `while` and `do`-`while` loops

-   `break` and `continue`

-   `switch` and `case`

-   `assert`

You can also affect the control flow using `try-catch` and `throw`, as
explained in [Exceptions](#exceptions).


### If and else

Dart supports `if` statements with optional `else` statements, as the
next sample shows. Also see [conditional expressions](#conditional-expressions).

<!-- language-tour/flow-if-else/bin/main.dart -->
{% prettify dart %}
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
{% endprettify %}

Remember, unlike JavaScript, Dart treats all values other than `true` as
`false`. See [Booleans](#booleans) for more information.


### For loops

You can iterate with the standard `for` loop. For example:

<!-- language-tour/flow/for-loops/web/main.dart -->
{% prettify dart %}
var message = new StringBuffer("Dart is fun");
for (var i = 0; i < 5; i++) {
  message.write('!');
}
{% endprettify %}

Closures inside of Dart’s `for` loops capture the value of the index,
avoiding a common pitfall found in JavaScript. For example, consider:

<!-- language-tour/flow/for-loops/web/main.dart -->
{% prettify dart %}
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}
callbacks.forEach((c) => c());
{% endprettify %}

The output is `0` and then `1`, as expected. In contrast, the example
would print `2` and then `2` in JavaScript.

If the object that you are iterating over is an Iterable, you can use the
[`forEach()`]({{site.dart_api}}/dart-core/Iterable/forEach.html)
method. Using `forEach()` is a good option if you don’t need to
know the current iteration counter:

<!-- language-tour/flow/for-loops/web/main.dart -->
{% prettify dart %}
candidates.forEach((candidate) => candidate.interview());
{% endprettify %}

Iterable classes such as List and Set also support the `for-in` form of
[iteration](/guides/libraries/library-tour#iteration):

<!-- language-tour/flow/for-loops/web/main.dart -->
{% prettify dart %}
var collection = [0, 1, 2];
for (var x in collection) {
  print(x);
}
{% endprettify %}


### While and do-while

A `while` loop evaluates the condition before the loop:

<!-- language-tour/flow-while/bin/main.dart -->
{% prettify dart %}
while (!isDone()) {
  doSomething();
}
{% endprettify %}

A `do`-`while` loop evaluates the condition *after* the loop:

<!-- language-tour/flow-while/bin/main.dart -->
{% prettify dart %}
do {
  printLine();
} while (!atEndOfPage());
{% endprettify %}


### Break and continue

Use `break` to stop looping:

<!-- language-tour/flow/break-continue/web/main.dart -->
{% prettify dart %}
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
{% endprettify %}

Use `continue` to skip to the next loop iteration:

<!-- language-tour/flow/break-continue/web/main.dart -->
{% prettify dart %}
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
{% endprettify %}

You might write that example differently if you’re using an
[Iterable]({{site.dart_api}}/dart-core/Iterable-class.html)
such as a list or set:

<!-- language-tour/flow/break-continue/web/main.dart -->
{% prettify dart %}
candidates.where((c) => c.yearsExperience >= 5)
          .forEach((c) => c.interview());
{% endprettify %}


### Switch and case

Switch statements in Dart compare integer, string, or compile-time
constants using `==`. The compared objects must all be instances of the
same class (and not of any of its subtypes), and the class must not
override `==`.
[Enumerated types](#enumerated-types) work well in `switch` statements.

<div class="alert alert-info" markdown="1">
**注意：**
Switch statements in Dart are intended for limited circumstances,
such as in interpreters or scanners.
</div>

Each non-empty `case` clause ends with a `break` statement, as a rule.
Other valid ways to end a non-empty `case` clause are a `continue`,
`throw`, or `return` statement.

Use a `default` clause to execute code when no `case` clause matches:

<!-- language-tour/flow-switch-case/bin/main.dart -->
{% prettify dart %}
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}
{% endprettify %}

The following example omits the `break` statement in a `case` clause,
thus generating an error:

<!-- language-tour/flow-switch-case/bin/main.dart -->
{% prettify dart %}
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // ERROR: Missing break causes an exception!!

  case 'CLOSED':
    executeClosed();
    break;
}
{% endprettify %}

However, Dart does support empty `case` clauses, allowing a form of
fall-through:

<!-- language-tour/flow-switch-case/bin/main.dart -->
{% prettify dart %}
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
{% endprettify %}

If you really want fall-through, you can use a `continue` statement and
a label:

<!-- language-tour/flow-switch-case/bin/main.dart -->
{% prettify dart %}
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
    // Continues executing at the nowClosed label.

nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
{% endprettify %}

A `case` clause can have local variables, which are visible only inside
the scope of that clause.


### Assert

Use an `assert` statement to disrupt normal execution if a boolean
condition is false. You can find examples of assert statements
throughout this tour. Here are some more:

<!-- language-tour/flow/assert/web/main.dart -->
{% prettify dart %}
// Make sure the variable has a non-null value.
assert(text != null);

// Make sure the value is less than 100.
assert(number < 100);

// Make sure this is an https URL.
assert(urlString.startsWith('https'));
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
Assert statements work only in checked mode. They have no effect in
production mode.
</div>

Inside the parentheses after `assert`, you can put any expression that
resolves to a boolean value or to a function. If the expression’s value
or function’s return value is true, the assertion succeeds and execution
continues. If it's false, the assertion fails and an exception (an
[AssertionError]({{site.dart_api}}/dart-core/AssertionError-class.html))
is thrown.


## Exceptions

Your Dart code can throw and catch exceptions. Exceptions are errors
indicating that something unexpected happened. If the exception isn’t
caught, the isolate that raised the exception is suspended, and
typically the isolate and its program are terminated.

In contrast to Java, all of Dart’s exceptions are unchecked exceptions.
Methods do not declare which exceptions they might throw, and you are
not required to catch any exceptions.

Dart provides
[Exception]({{site.dart_api}}/dart-core/Exception-class.html) and
[Error]({{site.dart_api}}/dart-core/Error-class.html)
types, as well as numerous predefined subtypes. You can, of course,
define your own exceptions. However, Dart programs can throw any
non-null object—not just Exception and Error objects—as an exception.

### Throw

Here’s an example of throwing, or *raising*, an exception:

<!-- language-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
throw new FormatException('Expected at least 1 section');
{% endprettify %}

You can also throw arbitrary objects:

<!-- language-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
throw 'Out of llamas!';
{% endprettify %}

Because throwing an exception is an expression, you can throw exceptions
in =\> statements, as well as anywhere else that allows expressions:

<!-- language-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
distanceTo(Point other) =>
    throw new UnimplementedError();
{% endprettify %}


### Catch

Catching, or capturing, an exception stops the exception from
propagating (unless you rethrow the exception).
Catching an exception gives you a chance to handle it:

<!-- lanaguage-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
{% endprettify %}

To handle code that can throw more than one type of exception, you can
specify multiple catch clauses. The first catch clause that matches the
thrown object’s type handles the exception. If the catch clause does not
specify a type, that clause can handle any type of thrown object:

<!-- language_tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
{% endprettify %}

As the preceding code shows, you can use either `on` or `catch` or both.
Use `on` when you need to specify the exception type. Use `catch` when
your exception handler needs the exception object.

You can specify one or two parameters to `catch()`.
The first is the exception that was thrown,
and the second is the stack trace
(a [StackTrace]({{site.dart_api}}/dart-core/StackTrace-class.html) object).

<!-- language-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
  ...
} on Exception catch [[highlight]](e)[[/highlight]] {
  print('Exception details:\n $e');
} catch [[highlight]](e, s)[[/highlight]] {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
{% endprettify %}

To partially handle an exception,
while allowing it to propagate,
use the `rethrow` keyword.

<!-- language-tour/reference/rethrow.dart -->
{% prettify dart %}
final foo = '';

void misbehave() {
  try {
    foo = "You can't change a final variable's value.";
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    [[highlight]]rethrow;[[/highlight]] // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
{% endprettify %}


### Finally

To ensure that some code runs whether or not an exception is thrown, use
a `finally` clause. If no `catch` clause matches the exception, the
exception is propagated after the `finally` clause runs:

<!-- language-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
try {
  breedMoreLlamas();
} finally {
  // Always clean up, even if an exception is thrown.
  cleanLlamaStalls();
}
{% endprettify %}

The `finally` clause runs after any matching `catch` clauses:

<!-- language-tour/flow/exceptions/web/main.dart -->
{% prettify dart %}
try {
  breedMoreLlamas();
} catch(e) {
  print('Error: $e');  // Handle the exception first.
} finally {
  cleanLlamaStalls();  // Then clean up.
}
{% endprettify %}

Learn more by reading the
[Exceptions](/guides/libraries/library-tour#exceptions) section.

## Classes

Dart is an object-oriented language with classes and mixin-based
inheritance. Every object is an instance of a class, and all classes
descend from [Object.]({{site.dart_api}}/dart-core/Object-class.html)
*Mixin-based inheritance* means that although every class (except for
Object) has exactly one superclass, a class body can be reused in
multiple class hierarchies.

To create an object, you can use the `new` keyword with a *constructor*
for a class. Constructor names can be either <code><em>ClassName</em></code> or
<code><em>ClassName</em>.<em>identifier</em></code>. For example:

<!-- language-tour/object-classes/bin/main.dart -->
{% prettify dart %}
var jsonData = JSON.decode('{"x":1, "y":2}');

// Create a Point using Point().
var p1 = new Point(2, 2);

// Create a Point using Point.fromJson().
var p2 = new Point.fromJson(jsonData);
{% endprettify %}

Objects have *members* consisting of functions and data (*methods* and
*instance variables*, respectively). When you call a method, you *invoke*
it on an object: the method has access to that object’s functions and
data.

Use a dot (`.`) to refer to an instance variable or method:

<!-- language-tour/object-classes/bin/main.dart -->
{% prettify dart %}
var p = new Point(2, 2);

// Set the value of the instance variable y.
p.y = 3;

// Get the value of y.
assert(p.y == 3);

// Invoke distanceTo() on p.
num distance = p.distanceTo(new Point(4, 4));
{% endprettify %}

Use `?.` instead of `.` to avoid an exception
when the leftmost operand is null:

{% comment %}
https://dartpad.dartlang.org/0cb25997742ed5382e4a
https://gist.github.com/0cb25997742ed5382e4a
{% endcomment %}

{% prettify dart %}
// If p is non-null, set its y value to 4.
p?.y = 4;
{% endprettify %}

Some classes provide constant constructors. To create a compile-time
constant using a constant constructor, use `const` instead of `new`:

<!-- language-tour/object-classes/bin/main.dart -->
{% prettify dart %}
var p = const ImmutablePoint(2, 2);
{% endprettify %}

Constructing two identical compile-time constants results in a single,
canonical instance:

<!-- language-tour/object-classes/bin/main.dart -->
{% prettify dart %}
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
{% endprettify %}

To get an object's type at runtime,
you can use Object's `runtimeType` property,
which returns a
[Type]({{site.dart_api}}/dart-core/Type-class.html) object.

<!-- language-tour/object-classes/bin/main.dart -->
{% prettify dart %}
print('The type of a is ${a.runtimeType}');
{% endprettify %}

The following sections discuss how to implement classes.


### Instance variables

Here’s how you declare instance variables:

<!-- language-tour/instance-variables/bin/main.dart -->
{% prettify dart %}
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
{% endprettify %}

All uninitialized instance variables have the value `null`.

All instance variables generate an implicit *getter* method. Non-final
instance variables also generate an implicit *setter* method. For details,
see [Getters and setters](#getters-and-setters).

<!-- language-tour/instance-variables/bin/main.dart -->
{% prettify dart %}
class Point {
  num x;
  num y;
}

main() {
  var point = new Point();
  point.x = 4;          // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
{% endprettify %}

If you initialize an instance variable where it is declared (instead of
in a constructor or method), the value is set when the instance is
created, which is before the constructor and its initializer list
execute.


### Constructors

Declare a constructor by creating a function with the same name as its
class (plus, optionally, an additional identifier as described in
[Named constructors](#named-constructors)).
The most common form of constructor, the generative constructor, creates
a new instance of a class:

<!-- language-tour/reference/constructor_long_way.dart -->
{% prettify dart %}
class Point {
  num x;
  num y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
{% endprettify %}

The `this` keyword refers to the current instance.

<div class="alert alert-info" markdown="1">
**注意：**
Use `this` only when there is a name conflict. Otherwise, Dart style
omits the `this`.
</div>

The pattern of assigning a constructor argument to an instance variable
is so common, Dart has syntactic sugar to make it easy:

<!-- language-tour/object-classes/bin/main.dart -->
{% prettify dart %}
class Point {
  num x;
  num y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
{% endprettify %}

#### Default constructors

If you don’t declare a constructor, a default constructor is provided
for you. The default constructor has no arguments and invokes the
no-argument constructor in the superclass.

#### Constructors aren’t inherited

Subclasses don’t inherit constructors from their superclass. A subclass
that declares no constructors has only the default (no argument, no
name) constructor.

#### Named constructors

Use a named constructor to implement multiple constructors for a class
or to provide extra clarity:

<!-- language-tour/bin/named-constructor.dart -->
{% prettify dart %}
class Point {
  num x;
  num y;

  Point(this.x, this.y);

  // Named constructor
  Point.fromJson(Map json) {
    x = json['x'];
    y = json['y'];
  }
}
{% endprettify %}

Remember that constructors are not inherited, which means that a
superclass’s named constructor is not inherited by a subclass. If you
want a subclass to be created with a named constructor defined in the
superclass, you must implement that constructor in the subclass.

#### Invoking a non-default superclass constructor

By default, a constructor in a subclass calls the superclass’s unnamed,
no-argument constructor.
The superclass's constructor is called at the beginning of the
constructor body. If an [initializer list](#initializer-list)
is also being used, it executes before the superclass is called.
In summary, the order of execution is as follows:

1. initializer list
1. superclass's no-arg constructor
1. main class's no-arg constructor

If the superclass doesn’t have an unnamed, no-argument constructor,
then you must manually call one of the constructors in the
superclass. Specify the superclass constructor after a colon (`:`), just
before the constructor body (if any).

In the following example, the constructor for the Employee class
calls the named constructor for its superclass, Person.
Click the run button ( {% img 'red-run.png' %} ) to execute the code.

<!-- language-tour/op-as/bin/main.dart -->
{% comment %}
https://gist.github.com/Sfshaza/e57aa06401e6618d4eb8
https://dartpad.dartlang.org/e57aa06401e6618d4eb8

class Person {
  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

main() {
  var emp = new Employee.fromJson({});
}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-dart-prefix}}?id=e57aa06401e6618d4eb8&horizontalRatio=99&verticalRatio=80"
    width="100%"
    height="500px"
    style="border: 1px solid #ccc;">
</iframe>

Because the arguments to the superclass constructor are evaluated before
invoking the constructor, an argument can be an expression such as a
function call:

<!-- language-tour/method-then-constructor/bin/main.dart -->
{% prettify dart %}
class Employee extends Person {
  // ...
  Employee() : super.fromJson(findDefaultData());
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
When using `super()` in a constructor's initialization list, put it last.
For more information, see the
[Dart usage guide](/guides/language/effective-dart/usage#do-place-the-super-call-last-in-a-constructor-initialization-list).
</div>

<div class="alert alert-warning" markdown="1">
**Warning:**
Arguments to the superclass constructor do not have access to `this`.
For example, arguments can call static methods but not instance methods.
</div>

#### Initializer list

Besides invoking a superclass constructor, you can also initialize
instance variables before the constructor body runs. Separate
initializers with commas.

<!-- language-tour/reference/initializer_list.dart -->
{% prettify dart %}
class Point {
  num x;
  num y;

  Point(this.x, this.y);

  // Initializer list sets instance variables before
  // the constructor body runs.
  Point.fromJson(Map jsonMap)
      : x = jsonMap['x'],
        y = jsonMap['y'] {
    print('In Point.fromJson(): ($x, $y)');
  }
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**Warning:**
The right-hand side of an initializer does not have access to `this`.
</div>

Initializer lists are handy when setting up final fields.
The following example initializes three final fields in an initializer list.
Click the run button ( {% img 'red-run.png' %} ) to execute the code.

<!-- language-tour/reference/initializer_list_final.dart -->
{% comment %}
https://gist.github.com/Sfshaza/7a9764702c0608711e08
https://dartpad.dartlang.org/7a9764702c0608711e08

import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(x, y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

main() {
  var p = new Point(2, 3);
  print(p.distanceFromOrigin);
}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-dart-prefix}}?id=7a9764702c0608711e08&horizontalRatio=99&verticalRatio=85"
    width="100%"
    height="420px"
    style="border: 1px solid #ccc;">
</iframe>

#### Redirecting constructors

Sometimes a constructor’s only purpose is to redirect to another
constructor in the same class. A redirecting constructor’s body is
empty, with the constructor call appearing after a colon (:).

<!-- language-tour/reference/along_x_axis.dart -->
{% prettify dart %}
class Point {
  num x;
  num y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
{% endprettify %}

#### Constant constructors

If your class produces objects that never change, you can make these
objects compile-time constants. To do this, define a `const` constructor
and make sure that all instance variables are `final`.

<!-- language-tour/reference/immutable_point.dart -->
{% prettify dart %}
class ImmutablePoint {
  final num x;
  final num y;
  const ImmutablePoint(this.x, this.y);
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);
}
{% endprettify %}

#### Factory constructors

Use the `factory` keyword when implementing a constructor that doesn’t
always create a new instance of its class. For example, a factory
constructor might return an instance from a cache, or it might return an
instance of a subtype.

The following example demonstrates a factory constructor returning
objects from a cache:

<!-- language-tour/factory-constructor/bin/main.dart -->
{% prettify dart %}
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to the _ in front
  // of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = new Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) {
      print(msg);
    }
  }
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
Factory constructors have no access to `this`.
</div>

To invoke a factory constructor, you use the `new` keyword:

<!-- language-tour/factory-constructor/bin/main.dart -->
{% prettify dart %}
var logger = new Logger('UI');
logger.log('Button clicked');
{% endprettify %}


### Methods

Methods are functions that provide behavior for an object.

#### Instance methods

Instance methods on objects can access instance variables and `this`.
The `distanceTo()` method in the following sample is an example of an
instance method:

<!-- language-tour/reference/distance_to.dart -->
{% prettify dart %}
import 'dart:math';

class Point {
  num x;
  num y;
  Point(this.x, this.y);

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
{% endprettify %}

#### Getters and setters

Getters and setters are special methods that provide read and write
access to an object’s properties. Recall that each instance variable has
an implicit getter, plus a setter if appropriate. You can create
additional properties by implementing getters and setters, using the
`get` and `set` keywords:

<!-- language-tour/rectangle/bin/main.dart -->
{% prettify dart %}
class Rectangle {
  num left;
  num top;
  num width;
  num height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right             => left + width;
      set right(num value)  => left = value - width;
  num get bottom            => top + height;
      set bottom(num value) => top = value - height;
}

main() {
  var rect = new Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
{% endprettify %}

With getters and setters, you can start with instance variables, later
wrapping them with methods, all without changing client code.

<div class="alert alert-info" markdown="1">
**注意：**
Operators such as increment (++) work in the expected way, whether or
not a getter is explicitly defined. To avoid any unexpected side
effects, the operator calls the getter exactly once, saving its value
in a temporary variable.
</div>

#### Abstract methods

Instance, getter, and setter methods can be abstract, defining an
interface but leaving its implementation up to other classes. To make a
method abstract, use a semicolon (;) instead of a method body:

<!-- language-tour/reference/doer.dart -->
{% prettify dart %}
abstract class Doer {
  // ...Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // ...Provide an implementation, so the method is not abstract here...
  }
}
{% endprettify %}

Calling an abstract method results in a runtime error.

Also see [Abstract classes](#abstract-classes).

#### Overridable operators

You can override the operators shown in the following table.
For example, if you define a
Vector class, you might define a `+` method to add two vectors.

`<`  | `+`  | `|`  | `[]`
`>`  | `/`  | `^`  | `[]=`
`<=` | `~/` | `&`  | `~`
`>=` | `*`  | `<<` | `==`
`–`  | `%`  | `>>`
{:.table}

Here’s an example of a class that overrides the `+` and `-` operators:

<!-- language-tour/vector/bin/main.dart -->
{% prettify dart %}
class Vector {
  final int x;
  final int y;
  const Vector(this.x, this.y);

  /// Overrides + (a + b).
  Vector operator +(Vector v) {
    return new Vector(x + v.x, y + v.y);
  }

  /// Overrides - (a - b).
  Vector operator -(Vector v) {
    return new Vector(x - v.x, y - v.y);
  }
}

main() {
  final v = new Vector(2, 3);
  final w = new Vector(2, 2);

  // v == (2, 3)
  assert(v.x == 2 && v.y == 3);

  // v + w == (4, 5)
  assert((v + w).x == 4 && (v + w).y == 5);

  // v - w == (0, 1)
  assert((v - w).x == 0 && (v - w).y == 1);
}
{% endprettify %}

If you override `==`, you should also override Object's `hashCode` getter.
For an example of overriding `==` and `hashCode`, see
[Implementing map keys](/guides/libraries/library-tour#implementing-map-keys).

For more information on overriding, in general, see
[Extending a class](#extending-a-class).


### Abstract classes

Use the `abstract` modifier to define an *abstract class*—a class that
can’t be instantiated. Abstract classes are useful for defining
interfaces, often with some implementation. If you want your abstract
class to appear to be instantiable, define a [factory
constructor](#factory-constructors).

Abstract classes often have [abstract methods](#abstract-methods).
Here’s an example of declaring an abstract class that has an abstract
method:

<!-- language-tour/reference/abstract.dart -->
{% prettify dart %}
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // ...Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
{% endprettify %}

The following class isn’t abstract, and thus can be instantiated even
though it defines an abstract method:

<!-- language-tour/reference/abstract.dart -->
{% prettify dart %}
class SpecializedContainer extends AbstractContainer {
  // ...Define more constructors, fields, methods...

  void updateChildren() {
    // ...Implement updateChildren()...
  }

  // Abstract method causes a warning but
  // doesn't prevent instantiation.
  void doSomething();
}
{% endprettify %}


### Implicit interfaces

Every class implicitly defines an interface containing all the instance
members of the class and of any interfaces it implements. If you want to
create a class A that supports class B’s API without inheriting B’s
implementation, class A should implement the B interface.

A class implements one or more interfaces by declaring them in an
`implements` clause and then providing the APIs required by the
interfaces. For example:

<!-- language-tour/imposter/bin/main.dart -->
{% prettify dart %}
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Imposter implements Person {
  // We have to define this, but we don't use it.
  final _name = "";

  String greet(who) => 'Hi $who. Do you know who I am?';
}

greetBob(Person person) => person.greet('bob');

main() {
  print(greetBob(new Person('kathy')));
  print(greetBob(new Imposter()));
}
{% endprettify %}

Here’s an example of specifying that a class implements multiple
interfaces:

<!-- language-tour/reference/point_interfaces.dart -->
{% prettify dart %}
class Point implements Comparable, Location {
  // ...
}
{% endprettify %}


### Extending a class

Use `extends` to create a subclass, and `super` to refer to the
superclass:

<!-- smart_tv.dart -->
{% prettify dart %}
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ...
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ...
}
{% endprettify %}

Subclasses can override instance methods, getters, and setters. Here’s
an example of overriding the Object class’s `noSuchMethod()` method,
which is called whenever code attempts to use a non-existent method or
instance variable:

<!-- language-tour/no-such-method/bin/main.dart -->
{% prettify dart %}
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  void noSuchMethod(Invocation mirror) {
    print('You tried to use a non-existent member:' +
          '${mirror.memberName}');
  }
}
{% endprettify %}

You can use the `@override` annotation to indicate that you are
intentionally overriding a member:

<!-- language-tour/overrides/bin/override.dart -->
{% prettify dart %}
class A {
  @override
  void noSuchMethod(Invocation mirror) {
    // ...
  }
}
{% endprettify %}

If you use `noSuchMethod()` to implement every possible getter, setter,
and method for one or more types,
then you can use the `@proxy` annotation to avoid warnings:

<!-- languager-tour/overrides/bin/proxy.dart -->
{% prettify dart %}
@proxy
class A {
  void noSuchMethod(Invocation mirror) {
    // ...
  }
}
{% endprettify %}

An alternative to `@proxy`, if you know the types at compile time,
is to just declare that the class implements those types.

<!-- language-tour/overrides/bin/proxy.dart -->
{% prettify dart %}
class A implements SomeClass, SomeOtherClass {
  void noSuchMethod(Invocation mirror) {
    // ...
  }
}
{% endprettify %}


For more information on annotations, see
[Metadata](#metadata).


<a id="enums"></a>
### Enumerated types

Enumerated types, often called _enumerations_ or _enums_,
are a special kind of class used to represent
a fixed number of constant values.


#### Using enums

Declare an enumerated type using the `enum` keyword:

<!-- language-tour/enum-switch/bin/main.dart -->
{% prettify dart %}
enum Color {
  red,
  green,
  blue
}
{% endprettify %}

Each value in an enum has an `index` getter,
which returns the zero-based position of the value in the enum declaration.
For example, the first value has index 0,
and the second value has index 1.

<!-- language-tour/enum-switch/bin/main.dart -->
{% prettify dart %}
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
{% endprettify %}

To get a list of all of the values in the enum,
use the enum's `values` constant.

<!-- language-tour/enum-switch/bin/main.dart -->
{% prettify dart %}
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
{% endprettify %}

You can use enums in [switch statements](#switch-and-case).
If the _e_ in <code>switch (<em>e</em>)</code> is explicitly typed as an enum,
then you're warned if you don't handle all of the enum's values:

<!-- language-tour/enum-switch/bin/main.dart -->
{% prettify dart %}
enum Color {
  red,
  green,
  blue
}
// ...
Color aColor = Color.blue;
switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor);  // 'Color.blue'
}
{% endprettify %}

Enumerated types have the following limits:

* You can't subclass, mix in, or implement an enum.
* You can't explicitly instantiate an enum.

For more information, see the
[Dart Language Specification](/guides/language/spec).


### Adding features to a class: mixins

Mixins are a way of reusing a class's code in multiple class
hierarchies.

To use a mixin, use the `with` keyword followed by one or more mixin
names. The following example shows two classes that use mixins:

<!-- language-tour/mixins/bin/main.dart -->
{% prettify dart %}
class Musician extends Performer with Musical {
  // ...
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
{% endprettify %}

To implement a mixin, create a class that extends Object,
declares no constructors, and has no calls to `super`. For example:

<!-- language-tour/mixins/bin/main.dart -->
{% prettify dart %}
abstract class Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
As of 1.13, two restrictions on mixins have been lifted
from the Dart VM:

* Mixins allow extending from a class other than Object.
* Mixins can call `super()`.

These "super mixins" are
[not yet supported in dart2js](https://github.com/dart-lang/sdk/issues/23773)
and require the `--supermixin` flag in dartanalyzer.
</div>

For more information, see the article [Mixins in
Dart.](/articles/language/mixins)


### Class variables and methods

Use the `static` keyword to implement class-wide variables and methods.

#### Static variables

Static variables (class variables) are useful for class-wide state and
constants:

<!-- language-tour/color/bin/main.dart -->
{% prettify dart %}
class Color {
  static const red =
      const Color('red'); // A constant static variable.
  final String name;      // An instance variable.
  const Color(this.name); // A constant constructor.
}

main() {
  assert(Color.red.name == 'red');
}
{% endprettify %}

Static variables aren’t initialized until they’re used.

<div class="alert alert-info" markdown="1">
**注意：**
This page follows the [style guide
recommendation](/guides/language/effective-dart/style#identifiers)
of preferring `lowerCamelCase` for constant names.
</div>

#### Static methods

Static methods (class methods) do not operate on an instance, and thus
do not have access to `this`. For example:

<!-- language-tour/point/main/bin.dart -->
{% prettify dart %}
import 'dart:math';

class Point {
  num x;
  num y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

main() {
  var a = new Point(2, 2);
  var b = new Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(distance < 2.9 && distance > 2.8);
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
Consider using top-level functions, instead of static methods, for
common or widely used utilities and functionality.
</div>

You can use static methods as compile-time constants. For example, you
can pass a static method as a parameter to a constant constructor.


## Generics

If you look at the API documentation for the basic array type,
[List,]({{site.dart_api}}/dart-core/List-class.html)
you’ll see that the
type is actually `List<E>`. The \<...\> notation marks List as a
*generic* (or *parameterized*) type—a type that has formal type
parameters. By convention, type variables have single-letter names, such
as E, T, S, K, and V.


### Why use generics?

Because types are optional in Dart, you never *have* to use generics.
You might *want* to, though, for the same reason you might want to use
other types in your code: types (generic or not) let you document and
annotate your code, making your intent clearer.

For example, if you intend for a list to contain only strings, you can
declare it as `List<String>` (read that as “list of string”). That way
you, your fellow programmers, and your tools (such as your IDE and
the Dart VM in checked mode) can detect that assigning a non-string to
the list is probably a mistake. Here’s an example:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
// ...
names.add(42); // Fails in checked mode (succeeds in production mode).
{% endprettify %}

Another reason for using generics is to reduce code duplication.
Generics let you share a single interface and implementation between
many types, while still taking advantage of checked mode and static
analysis early warnings. For example, say you create an interface for
caching an object:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
abstract class ObjectCache {
  Object getByKey(String key);
  setByKey(String key, Object value);
}
{% endprettify %}

You discover that you want a string-specific version of this interface,
so you create another interface:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
abstract class StringCache {
  String getByKey(String key);
  setByKey(String key, String value);
}
{% endprettify %}

Later, you decide you want a number-specific version of this
interface... You get the idea.

Generic types can save you the trouble of creating all these interfaces.
Instead, you can create a single interface that takes a type parameter:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
abstract class Cache<T> {
  T getByKey(String key);
  setByKey(String key, T value);
}
{% endprettify %}

In this code, T is the stand-in type. It’s a placeholder that you can
think of as a type that a developer will define later.


### Using collection literals

List and map literals can be parameterized. Parameterized literals are
just like the literals you’ve already seen, except that you add
<code>&lt;<em>type</em>></code> (for lists) or
<code>&lt;<em>keyType</em>, <em>valueType</em>></code> (for maps)
before the opening bracket. You might use
parameterized literals when you want type warnings in checked mode. Here
is example of using typed literals:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
{% endprettify %}


### Using parameterized types with constructors

To specify one or more types when using a constructor, put the types in
angle brackets (`<...>`) just after the class name. For example:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = new Set<String>.from(names);
{% endprettify %}

The following code creates a map that has integer keys and values of
type View:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
var views = new Map<int, View>();
{% endprettify %}


### Generic collections and the types they contain

Dart generic types are *reified*, which means that they carry their type
information around at runtime. For example, you can test the type of a
collection, even in production mode:

<!-- language-tour/generics/bin/main.dart -->
{% prettify dart %}
var names = new List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
{% endprettify %}

However, the `is` expression checks the type of the *collection*
only—not of the objects inside it. In production mode, a `List<String>`
might have some non-string items in it. The solution is to either check
each item’s type or wrap item-manipulation code in an exception handler
(see [Exceptions](#exceptions)).

<div class="alert alert-info" markdown="1">
**注意：**
In contrast, generics in Java use *erasure*, which means that generic
type parameters are removed at runtime. In Java, you can test whether
an object is a List, but you can’t test whether it’s a `List<String>`.
</div>


### Restricting the parameterized type

When implementing a generic type,
you might want to limit the types of its parameters.
You can do this using `extends`.

<!-- language-tour/generics-base-class/bin/main.dart -->
{% prettify dart %}
// T must be SomeBaseClass or one of its descendants.
class Foo<T [[highlight]]extends SomeBaseClass[[/highlight]]> {...}

class Extender extends SomeBaseClass {...}

void main() {
  // It's OK to use SomeBaseClass or any of its subclasses inside <>.
  var someBaseClassFoo = new Foo<SomeBaseClass>();
  var extenderFoo = new Foo<Extender>();

  // It's also OK to use no <> at all.
  var foo = new Foo();

  // Specifying any non-SomeBaseClass type results in a warning and, in
  // checked mode, a runtime error.
  // var objectFoo = new Foo<Object>();
}
{% endprettify %}


### Using generic methods

Initially, Dart's generic support was limited to classes.
A newer syntax, called _generic methods_, allows type arguments on methods and functions:

<!-- https://dartpad.dartlang.org/a02c53b001977efa4d803109900f21bb -->
<!-- https://gist.github.com/a02c53b001977efa4d803109900f21bb -->
{% prettify dart %}
[[highlight]]T[[/highlight]] first[[highlight]]<T>[[/highlight]](List<[[highlight]]T[[/highlight]]> ts) {
  // ...Do some initial work or error checking, then...
  [[highlight]]T[[/highlight]] tmp ?= ts[0];
  // ...Do some additional checking or processing...
  return tmp;
}
{% endprettify %}

Here the generic type parameter on `first` (`<T>`)
allows you to use the type argument `T` in several places:

* In the function's return type (`T`).
* In the type of an argument (`List<T>`).
* In the type of a local variable (`T tmp`).

<div class="alert alert-info" markdown="1">
**Version note:**
The new syntax for generic methods was [introduced in
SDK 1.21.](http://news.dartlang.org/2016/12/dart-121-generic-method-syntax.html)
If you use generic methods,
[specify an SDK version of 1.21 or higher.](/tools/pub/pubspec#sdk-constraints)
</div>

For more information about generics, see [Optional Types in
Dart](/articles/language/optional-types) and
[Using Generic Methods.](https://github.com/dart-lang/sdk/blob/master/pkg/dev_compiler/doc/GENERIC_METHODS.md)


## Libraries and visibility

The `import` and `library` directives can help you create a
modular and shareable code base. Libraries not only provide APIs, but
are a unit of privacy: identifiers that start with an underscore (\_)
are visible only inside the library. *Every Dart app is a library*, even
if it doesn’t use a `library` directive.

Libraries can be distributed using packages. See
[Pub Package and Asset Manager](/tools/pub)
for information about
pub, a package manager included in the SDK.


### Using libraries

Use `import` to specify how a namespace from one library is used in the
scope of another library.

For example, Dart web apps generally use the
[dart:html]({{site.dart_api}}/dart-html/dart-html-library.html)
library, which they can import like this:

<!-- language-tour/libraries/using_libraries.dart -->
{% prettify dart %}
import 'dart:html';
{% endprettify %}

The only required argument to `import` is a URI specifying the
library.
For built-in libraries, the URI has the special `dart:` scheme.
For other libraries, you can use a file system path or the `package:`
scheme. The `package:` scheme specifies libraries provided by a package
manager such as the pub tool. For example:

<!-- language-tour/libraries/using_schemes.dart, mylib, utils -->
{% prettify dart %}
import 'dart:io';
import 'package:mylib/mylib.dart';
import 'package:utils/utils.dart';
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
*URI* stands for uniform resource identifier.
*URLs* (uniform resource locators) are a common kind of URI.
</div>


#### Specifying a library prefix

If you import two libraries that have conflicting identifiers, then you
can specify a prefix for one or both libraries. For example, if library1
and library2 both have an Element class, then you might have code like
this:

<!-- language-tour/libraries/library_prefix.dart -->
{% prettify dart %}
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;
// ...
Element element1 = new Element();           // Uses Element from lib1.
lib2.Element element2 = new lib2.Element(); // Uses Element from lib2.
{% endprettify %}

#### Importing only part of a library

If you want to use only part of a library, you can selectively import
the library. For example:

<!-- language-tour/libraries/library_partial.dart, lib1, lib2 -->
{% prettify dart %}
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
{% endprettify %}

<a id="deferred-loading"></a>
#### Lazily loading a library

_Deferred loading_ (also called _lazy loading_)
allows an application to load a library on demand,
if and when it's needed.
Here are some cases when you might use deferred loading:

* To reduce an app's initial startup time.
* To perform A/B testing—trying out
  alternative implementations of an algorithm, for example.
* To load rarely used functionality, such as optional screens and dialogs.

To lazily load a library, you must first
import it using `deferred as`.

<!-- language-tour/deferred/bin/main.dart -->
{% prettify dart %}
import 'package:deferred/hello.dart' deferred as hello;
{% endprettify %}

When you need the library, invoke
`loadLibrary()` using the library's identifier.

<!-- language-tour/deferred/bin/main.dart -->
{% prettify dart %}
greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
{% endprettify %}

In the preceding code,
the `await` keyword pauses execution until the library is loaded.
For more information about `async` and `await`,
see [asynchrony support](#asynchrony-support).

You can invoke `loadLibrary()` multiple times on a library without problems.
The library is loaded only once.

Keep in mind the following when you use deferred loading:

* A deferred library's constants aren't constants in the importing file.
  Remember, these constants don't exist until the deferred library is loaded.
* You can't use types from a deferred library in the importing file.
  Instead, consider moving interface types to a library imported by
  both the deferred library and the importing file.
* Dart implicitly inserts `loadLibrary()` into the namespace that you define
  using <code>deferred as <em>namespace</em></code>.
  The `loadLibrary()` function returns a [Future](/guides/libraries/library-tour#future).

### Implementing libraries

See
[Create Library Packages](/guides/libraries/create-library-packages)
for advice on how to implement a library package.

<a id="asynchrony"></a>
## Asynchrony support

Dart has several language features
to support asynchronous programming.
The most commonly used of these features are
`async` functions and `await` expressions.

Dart libraries are full of functions that
return Future or Stream objects.
These functions are _asynchronous_:
they return after setting up
a possibly time-consuming operation
(such as I/O),
without waiting for that operation to complete.

When you need to use a value represented by a Future,
you have two options:

* Use `async` and `await`
* Use the [Future API](/guides/libraries/library-tour#future)

Similarly, when you need to get values from a Stream,
you have two options:

* Use `async` and an _asynchronous for loop_ (`await for`)
* Use the [Stream API](/guides/libraries/library-tour#stream)

Code that uses `async` and `await` is asynchronous,
but it looks a lot like synchronous code.
For example, here's some code that uses `await`
to wait for the result of an asynchronous function:

<!-- language-tour/async-await/bin/main.dart -->
{% prettify dart %}
await lookUpVersion()
{% endprettify %}

To use `await`, code must be in a function marked as `async`:

<!-- language-tour/async-await/bin/main.dart -->
{% prettify dart %}
checkVersion() async {
  var version = await lookUpVersion();
  if (version == expectedVersion) {
    // Do something.
  } else {
    // Do something else.
  }
}
{% endprettify %}

You can use `try`, `catch`, and `finally`
to handle errors and cleanup in code that uses `await`:

<!-- dart-tutorials-samples/httpserver/bin/mini_file_server.dart -->
{% prettify dart %}
try {
  server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4044);
} catch (e) {
  // React to inability to bind to the port...
}
{% endprettify %}


<a id="async"></a>
### Declaring async functions

An _async function_ is a function whose body is marked with
the `async` modifier.
Although an async function might perform time-consuming operations,
it returns immediately—before
any of its body executes.

<!-- language-tour/async-await/bin/main.dart -->
{% prettify dart %}
checkVersion() async {
  // ...
}

lookUpVersion() async => /* ... */;
{% endprettify %}

Adding the `async` keyword to a function makes it return a Future.
For example, consider this synchronous function,
which returns a String:

<!-- language-tour/async-await/bin/main.dart -->
{% prettify dart %}
String lookUpVersionSync() => '1.0.0';
{% endprettify %}

If you change it to be an async function—for example,
because a future implementation will be time consuming—the
returned value is a Future:

<!-- language-tour/async-await/bin/main.dart -->
{% prettify dart %}
Future<String> lookUpVersion() async => '1.0.0';
{% endprettify %}

Note that the function's body doesn't need to use the Future API.
Dart creates the Future object if necessary.


<a id="await"></a>
### Using await expressions with Futures

An await expression has the following form:

<pre>
<b>await</b> <em>expression</em>
</pre>

You can use `await` multiple times in an async function.
For example, the following code waits three times
for the results of functions:

<!-- library-tour/async-await/bin/main.dart -->
{% prettify dart %}
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
{% endprettify %}

In <code>await <em>expression</em></code>,
the value of <code><em>expression</em></code> is usually a Future;
if it isn't, then the value is automatically wrapped in a Future.
This Future object indicates a promise to return an object.
The value of <code>await <em>expression</em></code> is that returned object.
The await expression makes execution pause until that object is available.

**If `await` doesn't work, make sure it's in an async function.**
For example, to use `await` in your app's `main()` function,
the body of `main()` must be marked as `async`:

<!-- language-tour/async-await/bin/main.dart -->
{% prettify dart %}
main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
{% endprettify %}


<a id="await-for"></a>
### Using asynchronous for loops with Streams

An asynchronous for loop has the following form:

<pre>
<b>await for</b> (<em>variable declaration</em> in <em>expression</em>) {
  // Executes each time the stream emits a value.
}
</pre>

The value of <code><em>expression</em></code> must have type Stream.
Execution proceeds as follows:

1. Wait until the stream emits a value.
2. Execute the body of the for loop,
   with the variable set to that emitted value.
3. Repeat 1 and 2 until the stream is closed.

To stop listening to the stream,
you can use a `break` or `return` statement,
which breaks out of the for loop
and unsubscribes from the stream.

**If an asynchronous for loop doesn't work,
make sure it's in an async function.**
For example, to use an asynchronous for loop in your app's `main()` function,
the body of `main()` must be marked as `async`:

<!-- dart-tutorials-samples/httpserver/number_thinker.dart -->
{% prettify dart %}
main() async {
  ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  ...
}
{% endprettify %}

For more information about asynchronous programming, see the
[dart:async](/guides/libraries/library-tour#dartasync---asynchronous-programming)
section of the library tour.
Also see the articles
[Dart Language Asynchrony Support: Phase 1](/articles/language/await-async)
and
[Dart Language Asynchrony Support: Phase 2](/articles/language/beyond-async),
and the [Dart language specification](/guides/language/spec).

## Callable classes

To allow your Dart class to be called like a function,
implement the `call()` method.

In the following example, the `WannabeFunction` class defines
a call() function that takes three strings and concatenates them,
separating each with a space, and appending an exclamation.
Click the run button ( {% img 'red-run.png' %} ) to execute the code.

<!-- language-tour/callable-function/bin/main.dart -->
{% comment %}
https://gist.github.com/405379bacf30335f3aed
https://dartpad.dartlang.org/405379bacf30335f3aed

class WannabeFunction {
  call(String a, String b, String c) => a + ' ' + b + ' ' + c + '!';
}

main() {
  var wf = new WannabeFunction();
  var out = wf("Hi","there,","gang");
  print('$out');
}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-dart-prefix}}?id=405379bacf30335f3aed&horizontalRatio=99&verticalRatio=73"
    width="100%"
    height="240px"
    style="border: 1px solid #ccc;">
</iframe>

For more information on treating classes like functions, see
[Emulating Functions in Dart](/articles/language/emulating-functions).

## Isolates

Modern web browsers, even on mobile platforms, run on multi-core CPUs.
To take advantage of all those cores, developers traditionally use
shared-memory threads running concurrently. However, shared-state
concurrency is error prone and can lead to complicated code.

Instead of threads, all Dart code runs inside of *isolates*. Each
isolate has its own memory heap, ensuring that no isolate’s state is
accessible from any other isolate.


## Typedefs

In Dart, functions are objects, just like strings and numbers are
objects. A *typedef*, or *function-type alias*, gives a function type a
name that you can use when declaring fields and return types. A typedef
retains type information when a function type is assigned to a variable.

Consider the following code, which does not use a typedef:

<!-- language-tour/sorted-collection/bin/sorted_collection_broken.dart -->
{% prettify dart %}
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

 // Initial, broken implementation.
 int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}
{% endprettify %}

Type information is lost when assigning `f` to `compare`. The type of
`f` is `(Object, ``Object)` → `int` (where → means returns), yet the
type of `compare` is Function. If we change the code to use explicit
names and retain type information, both developers and tools can use
that information.

<!-- language-tour/sorted-collection/bin/sorted_collection_broken_2.dart -->
{% prettify dart %}
typedef int Compare(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

 // Initial, broken implementation.
 int sort(Object a, Object b) => 0;

main() {
  SortedCollection coll = new SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
Currently, typedefs are restricted to function types. We expect this
to change.
</div>

Because typedefs are simply aliases, they offer a way to check the type
of any function. For example:

<!-- language-tour/sorted_collection/bin/main.dart -->
{% prettify dart %}
typedef int Compare(int a, int b);

int sort(int a, int b) => a - b;

main() {
  assert(sort is Compare); // True!
}
{% endprettify %}


## Metadata

Use metadata to give additional information about your code. A metadata
annotation begins with the character `@`, followed by either a reference
to a compile-time constant (such as `deprecated`) or a call to a
constant constructor.

Three annotations are available to all Dart code: `@deprecated`,
`@override`, and `@proxy`. For examples of using `@override` and
`@proxy`, see [Extending a class](#extending-a-class).
Here’s an example of using the `@deprecated`
annotation:

<!-- language-tour/overrides/bin/main.dart -->
{% prettify dart %}
class Television {
  /// _Deprecated: Use [turnOn] instead._
  @deprecated
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {
    print('on!');
  }
}
{% endprettify %}

You can define your own metadata annotations. Here’s an example of
defining a @todo annotation that takes two arguments:

<!-- language-tour/meta-overrides/todo.dart -->
{% prettify dart %}
library todo;

class todo {
  final String who;
  final String what;

  const todo(this.who, this.what);
}
{% endprettify %}

And here’s an example of using that @todo annotation:

<!-- language-tour/meta-overrides/main.dart -->
{% prettify dart %}
import 'todo.dart';

@todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
{% endprettify %}

Metadata can appear before a library, class, typedef, type parameter,
constructor, factory, function, field, parameter, or variable
declaration and before an import or export directive. You can
retrieve metadata at runtime using reflection.


## Comments

Dart supports single-line comments, multi-line comments, and
documentation comments.


### Single-line comments

A single-line comment begins with `//`. Everything between `//` and the
end of line is ignored by the Dart compiler.

<!-- language-tour/single-line-comments/bin/main.dart -->
{% prettify dart %}
main() {
  // TODO: refactor into an AbstractLlamaGreetingFactory?
  print('Welcome to my Llama farm!');
}
{% endprettify %}


### Multi-line comments

A multi-line comment begins with `/*` and ends with `*/`. Everything
between `/*` and `*/` is ignored by the Dart compiler (unless the
comment is a documentation comment; see the next section). Multi-line
comments can nest.

<!-- language-tour/multi-line-comments/bin/main.dart -->
{% prettify dart %}
main() {
  /*
   * This is a lot of work. Consider raising chickens.

  Llama larry = new Llama();
  larry.feed();
  larry.exercise();
  larry.clean();
   */
}
{% endprettify %}


### Documentation comments

Documentation comments are multi-line or single-line comments that begin
with `///` or `/**`. Using `///` on consecutive lines has the same
effect as a multi-line doc comment.

Inside a documentation comment, the Dart compiler ignores all text
unless it is enclosed in brackets. Using brackets, you can refer to
classes, methods, fields, top-level variables, functions, and
parameters. The names in brackets are resolved in the lexical scope of
the documented program element.

Here is an example of documentation comments with references to other
classes and arguments:

<!-- language-tour/reference/doc-comments.dart -->
{% prettify dart %}
/// A domesticated South American camelid (Lama glama).
///
/// Andean cultures have used llamas as meat and pack
/// animals since pre-Hispanic times.
class Llama {
  String name;

  /// Feeds your llama [Food].
  ///
  /// The typical llama eats one bale of hay per week.
  void feed(Food food) {
    // ...
  }

  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}
{% endprettify %}

In the generated documentation, `[Food]` becomes a link to the API docs
for the Food class.

To parse Dart code and generate HTML documentation, you can use the SDK’s
[documentation generation tool.](https://github.com/dart-lang/dartdoc#dartdoc)
For an example of generated documentation, see the [Dart API
documentation.]({{site.dart_api}}) For advice on how to structure
your comments, see
[Guidelines for Dart Doc Comments.](/guides/language/effective-dart/documentation)


## Summary

This page summarized the commonly used features in the Dart language.
More features are being implemented, but we expect that they won’t break
existing code. For more information, see the [Dart Language
Specification](/guides/language/spec) and
[Effective Dart](/guides/language/effective-dart).

To learn more about Dart's core libraries, see
[A Tour of the Dart Libraries](/guides/libraries/library-tour).
