---
layout: guide
title: "Effective Dart: Style"
description: "Formatting and naming rules for consistent, readable code."

nextpage:
  url: /guides/language/effective-dart/documentation
  title: "文档"
prevpage:
  url: /guides/language/effective-dart
  title: "概述"
---

毋庸置疑好代码的代码风格一定是优雅的。一致的命名规则、一致的顺序、
以及一致的格式让代码*看起来*是一样的最终执行*就是一样*的。由于人眼识别
系统的工作方式，代码风格一样让我们更容易的阅读理解代码。如果在整个 Dart 语
言生态都使用一致的代码风格，那么所有的 Dart 从业人员都能从中获取到好处并且
可以很方便的参与到其他人的项目中去。

* TOC
{:toc}

## 标识符

Dart 中的标识符有三种风格。

*   `UpperCamelCase` 风格每个单词的首字母都大写，包含第一个单词。

*   `lowerCamelCase` 风格每个单词的首字母大写，*除了*第一个单词，第一个单词首字母小写。即使是首字符缩略词也是这样。

*   `lowercase_with_underscores` 风格只使用小写字母，即使是首字符缩略词也是这样，然后使用下划线 `_` 把每个单词分割开。


### 要 使用 `UpperCamelCase` 风格来命名类型名称

Classes（类名字）、 enums（枚举类型）、 typedefs（类型定义）、以及 type parameters（类型参数）应该把每个单词的首字母都大写，
(包含第一个单词)并且没有其他分隔符。

<div class="good" markdown="1">
{% prettify dart %}
class SliderMenu { ... }

class HttpRequest { ... }

typedef bool Predicate<T>(T value);
{% endprettify %}
</div>

用于注解的类也使用该类型命名。

<div class="good">
{% prettify dart %}
class Foo {
  const Foo([arg]);
}

@Foo(anArg)
class A { ... }

@Foo()
class B { ... }
{% endprettify %}
</div>

如果注解类使用的是无参数构造函数，则可以使用一个
`lowerCamelCase` 风格的常量来初始化这个注解。

<div class="good">
{% prettify dart %}
const foo = const Foo();

@foo
class C { ... }
{% endprettify %}
</div>


### 要 使用 `lowercase_with_underscores` 风格来命名库和文件名名字。

由于某些文件系统不区分大小写，所以很多项目都要求文件名必须为小写字母。
通过分隔符分开单词可以保证名字的可读性。使用下划线作为分隔符确保名字为
合法的 Dart 标识符，如果将来支持符号导入的话 文件名必须要为合法的标识符，
这样就不用将来再修改文件名了。

<div class="good">
{% prettify dart %}
library peg_parser.source_scanner;

import 'file_system.dart';
import 'slider_menu.dart';
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
library pegparser.SourceScanner;

import 'file-system.dart';
import 'SliderMenu.dart';
{% endprettify %}
</div>

注意，*如果你选择命名库的话*本指南规定了*如何*给库起名字。你也可以选择忽略
文件中的 library 指令。


### 要 使用 `lowercase_with_underscores` 风格命名导入的前缀。

<div class="good">
{% prettify dart %}
import 'dart:json' as json;
import 'dart:math' as math;
import 'package:javascript_utils/javascript_utils.dart' as js_utils;
import 'package:js/js.dart' as js;
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
import 'dart:json' as JSON;
import 'dart:math' as Math;
import 'package:javascript_utils/javascript_utils.dart' as jsUtils;
import 'package:js/js.dart' as JS;
{% endprettify %}
</div>


### 要 使用 `lowerCamelCase` 风格来命名其他的标识符。

类成员变量、顶级定义（变量、函数等）、变量、参数以及命名参数等都应该
使用 `lowerCamelCase` 这种类型的命名风格。
第一个单词首字母不大写并且没有分隔符。

<div class="good">
{% prettify dart %}
var item;

HttpRequest httpRequest;

align(clearItems) {
  // ...
}
{% endprettify %}
</div>


### 推荐 使用 `lowerCamelCase` 来命名常量。

在新的代码中使用 `lowerCamelCase` 来命名常量，包括枚举的值。
在已有的代码中你可以继续使用 `SCREAMING_CAPS` 风格来保持
代码的统一性。

<div class="good">
{% prettify dart %}
const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = new RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = new Random();
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
const PI = 3.14;
const kDefaultTimeout = 1000;
final URL_SCHEME = new RegExp('^([a-z]+):');

class Dice {
  static final NUMBER_GENERATOR = new Random();
}
{% endprettify %}
</div>

<aside class="alert alert-info" markdown="1">

**注意：** 一开始我们使用 Java 中的 `SCREAMING_CAPS` 风格来命名常量。后来我们不再使用
这种风格是由于：

*   `SCREAMING_CAPS` 在很多场合下看起来很费力，例如用于 CSS 中定义颜色
     的枚举值。

*   常量常常被修改为 final 类型的非常量变量，这种情况你还需要修改变量的
    名字为小写字母形式。

*   在枚举类型中自动定义的 `values` 属性为常量并且是小写字母
    形式的。

</aside>


### 要 把超过两个字母的缩略词和首字母缩略词当做一般单词来对待。

首字母缩略词都大写比较难以阅读，特别是多个首字母缩略词连在一起的时候。
例如 `HTTPSFTPConnection`、这种情况下你根本不知道到底是
 HTTPS FTP 链接还是 HTTP SFTP 链接。

为了避免这种情况，把超过两个字母的缩略词和首字母缩略词当做一般单词来对待。
(两个字符的 *缩写* 要和普通单词一样对待，例如 ID 和 Mr. 
只首字母大写，第二个字母小写。)

<div class="good">
{% prettify dart %}
HttpConnection
uiHandler
IOStream
HttpRequest
Id
DB
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
HTTPConnection
UiHandler
IoStream
HTTPRequest
ID
Db
{% endprettify %}
</div>


## 顺序

为了保证代码文件前面部分的整洁，我们规定了每个部分出现的顺序。
每个部分之间通过空行来分割。


### 要 把 "dart:" 导入语句放到其他导入语句之前。

<div class="good">
{% prettify dart %}
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
{% endprettify %}
</div>


### 要 把 "package:" 导入语句放到相对导入语句之前。

<div class="good">
{% prettify dart %}
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'a.dart';
{% endprettify %}
</div>


### 推荐 把"第三方" "package:" 导入语句放到其他语句之前。

如果你使用了多个 "package:" 导入语句来导入自己项目中的文件和第三方的文件，
推荐把自己的导入语句和其他的导入语句使用空行分开。

<div class="good">
{% prettify dart %}
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'package:myapp/io.dart';
import 'package:myapp/util.dart';
{% endprettify %}
</div>


### 要 把导出（export）语句放到所有导入语句之后的部分。

<div class="good">
{% prettify dart %}
import 'src/error.dart';
import 'src/string_source.dart';

export 'src/error.dart';
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
import 'src/error.dart';
export 'src/error.dart';
import 'src/string_source.dart';
{% endprettify %}
</div>


### 要 按照字母顺序来排序每个部分中的语句。

大部分 IDE 都可以自动完成这个操作。

<div class="good">
{% prettify dart %}
import 'package:bar/bar.dart';
import 'package:foo/bar.dart';

import 'a.dart';
import 'a/b.dart';
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
import 'package:foo/bar.dart';
import 'package:bar/bar.dart';

import 'a/b.dart';
import 'a.dart';
{% endprettify %}
</div>


## 格式化

和其他大部分语言一样， Dart 忽略空格。但是，*人*不这样。使用一致的
空格风格可以帮助人类按照机器的方式来
理解代码。

格式化代码是一个乏味的无聊任务，在重构的时候格式化代码还会消耗大量时间。
幸运的是，您无需手工来实现这项任务。我们提供了一个顶尖的自动化格式化
工具 -- [dartfmt][] 来自动实现这项无聊的任务。
Dart 中官方的空格处理规则由
*[dartfmt][] 生成*。

[dartfmt]: https://github.com/dart-lang/dart_style

### 避免 每行长度超过 80 字符。

阅读研究表明长文本眼睛需要费力的追踪文字换行，比较难以阅读。
这就是为何报纸纸张这么大，但是每页纸都分成很多短列来显示
内容。

如果你确实需要大于 80 个字符的代码行，我们的经验表明
您的代码很有可能太啰嗦了，可以变得更加简练。
最常见的的一种情况就是使用 `VeryLongCamelCaseClassNames`（非常长的类名字和变量名字）。
当你遇到这种情况，请自问一下：“是否每个单词都表达了一些关键信息并且防止出现
名字冲突的情况？”，如果回答是否，请考虑删除一些单词。

注意，dartfmt 能自动处理 99% 的情况，但是剩下的 1% 取决于您自己。 dartfmt 不会
把很长的字符串字面量分割为 80 个字符的列，所以这种情况你需要自己手工
确保每行不超过 80 个字符。

对于包含 URIs 的字符串则是一个例外&mdash;主要是导入和导出语句。
如果导入导出语句很长，则还是放到同一行上。
这样可以方便搜索某一个路径下的代码文件。


### 要 在所有的控制结构上使用大括号。

这样可以避免 [dangling else][] （悬挂else）的问题。

[dangling else]: http://en.wikipedia.org/wiki/Dangling_else

<div class="good">
{% prettify dart %}
if (true) {
  print('sanity');
} else {
  print('opposite day!');
}
{% endprettify %}
</div>

当只有 `if` 语句没有 `else` 语句并且
所有语句可以放到一行的时候，可以省略大括号。

<div class="good">
{% prettify dart %}
if (arg == null) return defaultValue;
{% endprettify %}
</div>

通常用于当条件满足的时候就跳出 if 或者 返回的情况。
但是对于其他表达式，如果可以放到一行中，
也可以这样使用。

<div class="good">
{% prettify dart %}
if (parameter == null) parameter = defaultValue;
{% endprettify %}
</div>


### 要 使用 `dartfmt` 来格式化您的代码

[dartfmt][] 自动使用下面所有的规则格式化代码，并且
还有其他的一些微妙的情况。自动化比手工要快很多并且还不会出错。
**如果你每次都使用 dartfmt 的话，下面的所有规则都不用阅读了。**


### 不要 使用 tabs 。

使用空格确保在所有的编辑器下代码看起来都是一致的。
并且还可以确保把代码发布到网站、博客等地方看起来也是一致的，
例如  [GitHub](https://github.com/)。

现代的编辑器可以配置 tab 键的行为，使用空格来代替 tab，这样
可以帮你处理空格的一致性问题。


### 要 在每个语句或者声明后面添加一个空行。

<div class="good">
{% prettify dart %}
main() {
  first(statement);
  second(statement);
}

anotherDeclaration() { ... }
{% endprettify %}
</div>


### 不要 在声明函数名字、操作符(operator)和 setter 以及参数名字之间添加空格。

<div class="good">
{% prettify dart %}
bool convertToBool(arg) { ... }
bool operator ==(other) { ... }
set contents(value) { ... }
{% endprettify %}
</div>


### 要 在关键字 `operator` 后面添加一个空格。

<div class="good">
{% prettify dart %}
bool operator ==(other) => ...;
{% endprettify %}
</div>


### 要 在二元和三元操作符之间添加空格。

注意， `<` 和 `>` 在表达式中使用的时候被当做二元操作符，
当作为泛型使用的时候不是。`is` 和 `is!` 都被当做一个二元操作符。
然而，`.` 用来访问成员变量的时候不是二元操作符并且不
应该添加空格。

<div class="good">
{% prettify dart %}
average = (a + b) / 2;
largest = a > b ? a : b;
if (obj is! SomeType) print('not SomeType');
optional([parameter = defaultValue]) { ... }
{% endprettify %}
</div>


### 要 在 `,` 和 `:` 后面添加空格，当用作 map 和命名参数的情况下。

<div class="good">
{% prettify dart %}
function(a, b, named: c);
[some, list, literal];
{map: literal}
{% endprettify %}
</div>


### 不要 在一元操作符前后添加空格。

<div class="good">
{% prettify dart %}
!condition
index++
{% endprettify %}
</div>


### 要在 `in` 关键字前后添加空格，在循环中的每个 `;` 后面也要添加空格。

<div class="good">
{% prettify dart %}
for (var i = 0; i < 100; i++) ...

for (final item in collection) ...
{% endprettify %}
</div>


### 要 在流程控制关键字后面使用一个空格。

这条和函数以及方法调用不同，函数和方法调用
*无需*在名字和括号之间添加空格。

<div class="good">
{% prettify dart %}
while (foo) ...

try {
  // ...
} catch (e) {
  // ...
}
{% endprettify %}
</div>


### 不要在 `(`, `[`, 和 `{` 之后使用空格，也不要在 `)`, `]`, 和 `}` 之前使用空格。

当 `<` 和 `>` 用于泛型的时候也不要使用空格。

<div class="good">
{% prettify dart %}
var numbers = <int>[1, 2, (3 + 4)];
{% endprettify %}
</div>


### 要 在函数和方法体的 `{` 之前添加一个空格。

当 `{` 用于方法或者函数参数后，在和参数结尾的  `)` 之间
应该添加一个空格。

<div class="good">
{% prettify dart %}
getEmptyFn(a) {
  return () {};
}
{% endprettify %}
</div>


### 要 把开始的大括号 (`{`) 放到同一行上。

<div class="good">
{% prettify dart %}
class Foo {
  method() {
    if (true) {
      // ...
    } else {
      // ...
    }
  }
}
{% endprettify %}
</div>


### 要 把二元符合放到多行表达式的前面一行的结尾。

虽然使用哪种风格都可以，但是为了统一性，我们通常使用
这种方式。

<div class="good">
{% prettify dart %}
var bobLikesIt = isDeepFried ||
    (hasPieCrust && !vegan) ||
    containsBacon;

bobLikes() =>
    isDeepFried || (hasPieCrust && !vegan) || containsBacon;
{% endprettify %}
</div>


### 要 把三元操作符放到多个表达式的下一行开始位置。

如果你在其中一个操作符之前分行，请在另外一个操作符之前也分行。

<div class="good">
{% prettify dart %}
return someCondition
    ? whenTrue
    : whenFalse;
{% endprettify %}
</div>


### 要 把 `.` 放到下一行开头当表达式换行的时候。

<div class="good">
{% prettify dart %}
someVeryLongVariable.withAVeryLongProperty
    .aMethodOnThatObject();
{% endprettify %}
</div>


### 要 把构造函数初始化列表中的每个参数和值都放到同一行。

<div class="good">
{% prettify dart %}
MyClass()
    : firstField = 'some value',
      secondField = 'another',
      thirdField = 'last' {
  // ...
}
{% endprettify %}
</div>

注意 `:`  应该放到下一行并且缩进四个空格。
所有的参数应该对齐（所以每个参数缩进
六个空格）。


### 推荐 当无法在一行写完集合的时候，把每个元素都用集合定义的方式来表达。（PREFER splitting every element in a collection literal if it does not fit on one line.）

在意味着在开始的括号之后和关闭括号之前以及每个
元素之后的 `,`。

<div class="good">
{% prettify dart %}
mapInsideList([
  {
    'a': 'b',
    'c': 'd'
  },
  {
    'a': 'b',
    'c': 'd'
  },
]);
{% endprettify %}
</div>


### 要 用两个空格来缩进代码块和集合体。

<div class="good">
{% prettify dart %}
if (condition) {
  print('hi');

  [
    long,
    list,
    literal
  ];
}
{% endprettify %}
</div>


### 要 缩进 switch case 两个空格， case 体四个空格。

<div class="good">
{% prettify dart %}
switch (fruit) {
  case 'apple':
    print('delish');
    break;

  case 'durian':
    print('stinky');
    break;
}
{% endprettify %}
</div>


### 要 只少使用两个空格来缩进多行函数级联调用。

<div class="good">
{% prettify dart %}
buffer
  ..write('Hello, ')
  ..write(name)
  ..write('!');
{% endprettify %}
</div>


### 推荐 使用四个空格来缩进同一行的换行。

<div class="good">
{% prettify dart %}
someLongObject.aReallyLongMethodName(longArg, anotherLongArg,
    wrappedToNextLine);
{% endprettify %}
</div>

这也包含 `=>` ：

<div class="good">
{% prettify dart %}
bobLikes() =>
    isDeepFried || (hasPieCrust && !vegan) || containsBacon;
{% endprettify %}
</div>

这也有些例外情况，当表达式包含多行函数或者
集合声明定义的时候除外。

<div class="good">
{% prettify dart %}
new Future.delayed(const Duration(seconds: 1), () {
  print('I am a callback');
});

args.addAll([
  '--mode',
  'release',
  '--checked'
]);
{% endprettify %}
</div>

你的目标是平衡使用缩进来显示表达式的结构但是
又不想大量的没必要的缩进代码。（Your goal is to balance using indentation to show expression structure while
not wanting to indent large swathes of code unecessarily.）

