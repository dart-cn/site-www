---
layout: default
title: "Dart 核心库预览"
description: "学习 Dart 的核心库和其 API。"
permalink: /guides/libraries
short-title: "库"
toc: false
---

Dart 程序离不开各种 _库_。
一些常见的库已经可以使用了。
例如，`dart:core` 提供了少量但是非常重要的一些内置功能，
例如 numbers、集合以及 strings。另外一个必要的库是
`dart:async`，用来支持异步编程，里面定义了
像 Future 和 Stream 这样的类。
通过从包中导入其他的库，你可以使用很多第三方库的功能。

下面是一些关于如何使用、创建和分析 Dart 库和包
的内容。

<div class="card-grid">
  <div class="card">
    <h3><a href="/guides/libraries/library-tour">Dart 库预览</a></h3>
    <p>介绍 Dart 语言核心库中的主要特性。</p>
  </div>

  <div class="card">
    <h3><a href="/guides/libraries/useful-libraries">常用的库</a></h3>
    <p>一些你应该了解的常用库。</p>
  </div>

  <div class="card">
    <h3><a href="/guides/libraries/create-library-packages">创建一个库</a></h3>
    <p>如何创建自己的库。</p>
  </div>
</div>

## Other resources（其他资源）

[安装共享的包](/tutorials/libraries/shared-pkgs)
: 一个告诉你如何安装并使用第三方库的新手教程。

[哪些文件不要提交到代码库中](/guides/libraries/private-files)
: 了解哪些你的开发工具生成的文件是不需要提交到代码库中
  的。

[pub](/tools/pub)
: 一个管理 Dart 包的工具。

[pub.dartlang.org](https://pub.dartlang.org/)
: Dart 社区发布的各种第三方库。

[关于库的文章](/articles/libraries)
: 各种关于库的文章。

API 参考文档
: * [api.dartlang.org]({{site.dart_api}}):
    自动生成的 dart:* 库的文档
  * [docs.flutter.io](http://docs.flutter.io/):
    自动生成的 Flutter 的文档
  * [dartdocs.org](https://www.dartdocs.org/):
    自动生成的发布在 pub.dartlang.org 上的各种库的文档
