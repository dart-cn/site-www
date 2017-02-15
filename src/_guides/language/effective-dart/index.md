---
layout: guide
title: "Effective Dart"
description: "构建一致的、可维护的、高效率 Dart 应用的最佳实践。"
permalink: /guides/language/effective-dart
toc: true

nextpage:
  url: /guides/language/effective-dart/style
  title: "代码风格"
---

在过去的几年中，我们写了很多 Dart 代码，从中了解到应该如何做以及不应该如何做。
这里我们把学习到的经验分享给你，这样你也可以
很轻松的写出一致的、健壮的、高效率的代码。


下面是编写有效代码的两个主要主题：

 1. **要保持一致。** 像代码格式化、标识符大小写、以及参数命名等最好保持一致。
    虽然这些东西都有很多主管意愿在里面，但是我们的经验告诉我们保持一致
    是客观有帮助的。

    如果两块代码看起来不一样，则他们所实现的功能就应该有区别。
    当一块代码和其他的表现不一样并且吸引了你的注意力，
    则这块代码应该有一个这样做的理由。

 2. **要保持精简。** Dart 设计的时候就吸取了其他语言（Java 、C JavaScript 等）
    的各种优点， Dart 非常容易使用。
    但是我们之所以创建 Dart 语言是因为这些语言都有一些缺点， Dart 作为一个新的语言
    可以吸取他们的经验来提供更好的语言特性支持。我们提供了很多新的特性，
    比如 字符串插值、初始化范式等可以使用更加简单
    和直观的方式来表达你的意图。

    如果有多种方式可以完成同一个功能，则你应该选择最精简的那种表达方式。
    但是这并不意味着你需要像 [code golf][]（代码高尔夫挑战赛）一样把代码
    写的越短越好。目标是让代码直观、易于读写。

[code golf]: https://en.wikipedia.org/wiki/Code_golf

## 指南概述

为了便于理解我们把指南分为了几个部分：

  * **[代码风格指南][]** &ndash; 规定了如何格式化代码&mdash;
    [dartfmt] 的实现使用同样的规则。同样还指定了如何
    格式化标识符：`camelCase`、 `using_underscores`、等。

  * **[文档注释指南][]** &ndash; 告诉你关于如何编写注释文档的一切内容。
    包含代码文档注释、
    普通注释。

  * **[最佳实践指南][]** &ndash; 告诉你如何使用语言提供的特性来实现你的功能。
    如果是关于语句和表达式的，则
    会在这里介绍。

  * **[API设计指南][]** &ndash; 这个指南最宽松，但是应用面最广。
    涵盖了如何设计一致的、可用的 API。
    关于方法返回何种类型、如何定义
    等内容在这里会介绍。

所以指南的条目索引请查看
[大纲](#大纲)。

<aside class="alert alert-info" markdown="1">
  **Have feedback on the guides?** <br>
  Please create a [new issue][] or comment on one of our [existing issues][].

  [new issue]: https://github.com/dart-lang/site-www/issues/new
  [existing issues]: https://github.com/dart-lang/site-www/issues?q=is%3Aopen+is%3Aissue+label%3AEffectiveDart
</aside>

[dartfmt]: https://github.com/dart-lang/dart_style#readme
[代码风格指南]: /guides/language/effective-dart/style
[文档注释指南]: /guides/language/effective-dart/documentation
[最佳实践指南]: /guides/language/effective-dart/usage
[API设计指南]: /guides/language/effective-dart/design

## 如何阅读本指南

每个指南都分为了几个部分。每个部分包含一些详细的指导条目。
每个指导条目以下面其中的一个词开头：

* **要** 开头所表述的内容是要准守的。
从来不会有合理的理由来违反这些条目。

* **不要** 开头所表述的内容是相反的：
这样做从来不是一个好主意。幸运的是，在 Dart 中只有很少几条这样的规则。
其他语言由于历史包袱原因会有很多类似的规则。
Dart 是新的语言，所以在语言设计之初，我们就可以直接修复
这些问题。

* **推荐** 开头所表述的内容你*应该*准守。
但是在有些情况下，可能有更好的或者更合理的做法。
当你不遵守这些条目的时候，请确保你有合理的
理由这么做。

* **避免** 开头所表述的内容和“推荐”相反：
如果没有足够好的理由，你不应该这么做。

* **考虑** 开头所表述的内容你可以遵守，
也可以不准守，根据你的实际情况来选择。

这听起来比较严肃。
其实并没有这么糟糕。
大部分的条目都是常识，并且我们都是理性的人，
我们的目标都是一致的：编写出优雅的、易于阅读的、可维护的代码。

## 词汇表

为了保持指导手册的简洁，我们使用了一些缩写词语来引用
Dart 中的元素。

* 一个 **成员** 是定义在库中的任何内容。
  包括顶级变量、getter、setter以及方法。
  还包括类中的静态成员以及实例成员：成员变量（fields）、
  setter、getter、方法以及操作符。

* 一个 **变量** 是指任意的变量声明，局部变量或者顶级变量。
  还包括成员参数。

* 一个 **类型** 是任何命名类型的声明：一个类、 typedef、或者 enum。

* 一个 **属性** 是和变量类似的构造。
  包括类中的成员变量、getter 和 setter。
  还包括顶级变量、getter 和 setter。

> 译注：在指南翻译中为了方便理解，有些词语会按照中文上下文做些调整。比如成员有时候就是具体指类的函数，方便理解会翻译为函数。

## 大纲

{% include_relative toc.md %}
