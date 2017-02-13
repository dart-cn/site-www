---
layout: guide
title: "Effective Dart: 文档"
description: "清晰的、有帮助的注释和文档。"

nextpage:
  url: /guides/language/effective-dart/usage
  title: "最佳实践"
prevpage:
  url: /guides/language/effective-dart/style
  title: "代码风格"
---

如果您已经知道了代码的上下文信息，则更容易理解代码。
但是当新手阅读您的代码或者很久以后您再次阅读您的代码，
代码的上下文信息可能已经不记得了。编写精炼的、准确的注释
只需要几秒钟，但是以后可能节省其他人几个小时
的时间来读懂您的代码。

我们已经知道代码应该自我说明（代码就是最好的文档）并且并非所有的注释都是有用的。
但是，真实情况是大家都把应该写的评论给省略了。
和练习一样：从技术上你**可以**做很多，但是实际上你
只做了一点点。尝试着逐步提高。

* TOC
{:toc}

## 注释

下面的说明应用于不包含在生成的
代码文档中的注释。

### **要** 按照句子的格式来格式化评论。

<div class="good">
{% prettify dart %}
// Not if there is nothing before it.
if (_chunks.isEmpty) return false;
{% endprettify %}
</div>

如果第一个单词不是大小写相关的标识符，则首字母要大写。使用标点符号结尾
（句号、感叹号、问号）。对于所有的注释都是这样要求的：文档注释、
行内注释、甚至 TODO 注释。即使是一句话的一半也需要如此。

### **不要** 使用块注释作为解释说明。

<div class="good">
{% prettify dart %}
greet(name) {
  // Assume we have a valid name.
  print('Hi, $name!');
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
greet(name) {
  /* Assume we have a valid name. */
  print('Hi, $name!');
}
{% endprettify %}
</div>

你可以使用块注释 (`/* ... */`) 来临时的注释掉一些代码，
但是其他的所有注释都应该使用 `//`。

## 文档注释

文档注释非常有用，[dartdoc][] 解析这些注释并生成
[漂亮的文档网页][docs] 。出现在定义语句（变量定义、函数声明、类定义 等）之前并且使用
 `///` 语法的都是文档注释。

[dartdoc]: https://github.com/dart-lang/dartdoc
[docs]: {{site.dart_api}}

### **要** 使用 `///` 文档注释来注释成员和类型。

使用文档注释可以让 [dartdoc][] 来为你生成
代码 API 文档。

<div class="good">
{% prettify dart %}
/// The number of characters in this chunk when unsplit.
int get length => ...
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
// The number of characters in this chunk when unsplit.
int get length => ...
{% endprettify %}
</div>

由于历史原因，dartdoc 支持两种格式的文档注释：
`///` ("C# 格式") 和 `/** ... */` ("JavaDoc 格式")。我们推荐使用 `///` 
是因为其更加简洁。`/**` 和 `*/` 在多行注释中间添加了开头和结尾的两行多余
内容。 `///` 在一些情况下也更加易于阅读，例如
当注释文档中包含有使用 `*` 标记的列表内容的时候。

如果你现在还在使用 JavaDoc 风格格式，请考虑
使用新的格式。

### **推荐** 为公开发布的 API 编写注释文档。

你没必要为所有顶级的变量、类型、成员 都提供注释文档，但是
对于公开的成员则应该提供注释文档。

### **考虑** 为私有 API 编写注释文档。

文档不仅仅为了使用你的类的用户所编写。
也可以用来帮助理解你的类是如何调用其他
类的。

### **要** 把第一个语句定义为一个段落。

注释文档中的第一个段落应该是简洁的、面向用户的注释。例如下面的示例，
通常不是一个完成的语句。

<div class="good">
{% prettify dart %}
/// Defines a flag.
///
/// Throws an [ArgumentError] if there is already an option named [name] or
/// there is already an option using abbreviation [abbr]. Returns the new flag.
Flag addFlag(String name, String abbr) { ... }
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
/// Starts a new block as a child of the current chunk. Nested blocks are
/// handled using their own independent [LineWriter].
ChunkBuilder startBlock() { ... }
{% endprettify %}
</div>

描述部分应该告诉读者这个 API 可以提供哪些功能，和类似的 API 标示
其区别。不要只是
重复 API 的名字，要告诉读者一些他们不知道的信息。

### **推荐** 用第三人称来开始函数或者方法的文档注释。

注释应该关注于代码 *所实现的* 功能。

<div class="good">
{% prettify dart %}
/// Returns `true` if every element satisfies the [predicate].
bool all(bool predicate(T element)) { ... }

/// Starts the stopwatch if not already running.
void start() { ... }
{% endprettify %}
</div>

### **推荐** 使用名词短语来开始变量、getter、setter 的注释。

注释文档应该表述这个属性*是*什么。虽然 getter 函数会做些计算，
 但是也要求这样，调用者关心的是其*结果*而
 不是如何计算的。

<div class="good">
{% prettify dart %}
/// The current day of the week, where `0` is Sunday.
int weekday;

/// The number of checked buttons on the page.
int get checkedCount { ... }
{% endprettify %}
</div>

如果同时有 getter 和 setter，只需要在 getter 上添加注释。
这样 dartdoc 会按照变量来对待 getter 和 setter。

### **推荐** 使用名词短语来开始库和类型注释。

在程序中，类的注释通常是最重要的文档。
类的注释描述了类型的不变性、介绍其使用的术语、
提供类成员使用的上下文信息。为类提供一些注释可以让
其他类成员的注释更易于理解和编写。

<div class="good">
{% prettify dart %}
/// A chunk of non-breaking output text terminated by a hard or soft newline.
///
/// ...
class Chunk { ... }
{% endprettify %}
</div>

### **考虑** 在文档注释中添加示例代码。

<div class="good">
{% prettify dart %}
/// Returns the lesser of two numbers.
///
///     min(5, 3); // 3.
num min(num a, num b) { ... }
{% endprettify %}
</div>

人类非常擅长从示例中抽象出实质内容，所以即使提供
一行最简单的示例代码都可以让 API 更易于理解。

### **要** 使用方括号在文档注释中引用作用域内的标识符。

如果把变量、函数、类型名字放到方括号里面，则 dartdoc
在生成文档的时候会链接到这些成员。

<div class="good">
{% prettify none %}
Throws a [StateError] if ...

similar to [anotherMethod], but ...
{% endprettify %}
</div>

在标识符前面添加 new 关键字可以链接构造函数。

<div class="good">
{% prettify none %}
To create a point, call [new Point] or use [new Point.polar] to ...
{% endprettify %}
</div>

### **要** 使用散文的方式来描述参数、返回值以及异常信息。

其他语言使用各种标签和额外的注释来描述参数和
返回值。

<div class="bad">
{% prettify dart %}
/// Defines a flag with the given name and abbreviation.
///
/// @param name The name of the flag.
/// @param abbr The abbreviation for the flag.
/// @returns The new flag.
/// @throws ArgumentError If there is already an option with
///     the given name or abbreviation.
Flag addFlag(String name, String abbr) { ... }
{% endprettify %}
</div>

而 Dart 把参数、返回值等描述放到文档注释中，并使用方括号来引用
以及高亮这些参数和返回值。

<div class="good">
{% prettify dart %}
/// Defines a flag.
///
/// Throws an [ArgumentError] if there is already an option named [name] or
/// there is already an option using abbreviation [abbr]. Returns the new flag.
Flag addFlag(String name, String abbr) { ... }
{% endprettify %}
</div>

### **避免** 在注释文档中提供冗余的类型信息。

用户在阅读文档注释的时候可以查看类型、返回值类型以及参数类型。并且在文档中
还可以跳转到变量的定义地方。所以
根本没必要在注释中加上额外的类型信息。

要告诉读者他们 *不知道* 的信息。

### **要** 把注释文档放到注解之前。

<div class="good">
{% prettify dart %}
/// _Deprecated: Use [newMethod] instead._
@deprecated
oldMethod();
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
@deprecated
/// _Deprecated: Use [newMethod] instead._
oldMethod();
{% endprettify %}
</div>


## Markdown

在注释文档中可以使用大部分 [markdown][] 格式，
dartdoc 会使用 [markdown package][] 来格式化这些内容。

[markdown]: https://daringfireball.net/projects/markdown/
[markdown package]: https://pub.dartlang.org/packages/markdown

现在到处都有介绍 Markdown 的文档，以及到处都是使用
Markdown。所以我们选择了支持它。
下面是一个 Markdown 语法的快速预览。

{% prettify dart %}
/// This is a paragraph of regular text.
///
/// This sentence has *two* _emphasized_ words (i.e. italics) and **two**
/// __strong__ ones (bold).
///
/// A blank line creates another separate paragraph. It has some `inline code`
/// delimited using backticks.
///
/// * Unordered lists.
/// * Look like ASCII bullet lists.
/// * You can also use `-` or `+`.
///
/// 1. Numbered lists.
/// 2. Are, well, numbered.
/// 1. But the values don't matter.
///
///     * You can nest lists too.
///     * They must be indented at least 4 spaces.
///     * (Well, 5 including the space after `///`.)
///
/// Code blocks are indented the same way:
///
///     this.code
///         .will
///         .retain(its, formatting);
///
/// Links can be:
///
/// * http://www.just-a-bare-url.com
/// * [with the URL inline](http://google.com)
/// * [or separated out][ref link]
///
/// [ref link]: http://google.com
///
/// # A Header
///
/// ## A subheader
///
/// ### A subsubheader
///
/// #### If you need this many levels of headers, you're doing it wrong
{% endprettify %}

### **避免** 过度使用 markdown。

当你不确定的时候，就不要使用文字格式。文字格式是为了更好的表达你的意图，
而不是替代你的文字内容。 文字内容才是最重要的。

### **避免** 使用 HTML 来格式化文字。

在极少数情况下使用HTML *可能是* 有用的。比如显示表格。但是在大部分
 情况下都没必要使用它。和 Markdown 相比太复杂了，最好
 不要使用 HTML。

## 如何写注释

虽然我们认为我们是程序员，但是大部分情况下源代码中的内容都是为了
让人类更易于阅读和理解代码。对于
任何编程语言，都值得
努力练习来提高您的熟练程度。

下面列举了一些编写文档的指导原则。在
[技术写作风格](https://en.wikiversity.org/wiki/Technical_writing_style) 中你可以了解
技术写作的最佳实践。

### **推荐** 简洁.

要清晰和准确，同时还要简洁。

### **避免** 缩写和首字母缩略词（非常流行的除外）。

大部分人都不知道 "i.e."、 "e.g." 和 "et. al." 的意思。你所在领域的
首字母缩略词对于其他人可能并不了解。

### **推荐** 使用 "this" 而不是 "the" 来引用实例成员。

当提及到类的成员，通常是指被调用的对象实例的成员。
使用 "the" 可能会导致混淆。

<div class="good">
{% prettify dart %}
class Box {
  /// The value this wraps.
  var _value;

  /// True if this box contains a value.
  bool get hasValue => _value != null;
}
{% endprettify %}
</div>

