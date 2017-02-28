---
layout: default
title: "常用的库"
description: "一些你应该了解的常用库。"
---

在写代码的时候你可以使用很多 Dart 库。
Dart 库来源于很多地方：

* 核心库&mdash;例如 dart:core、 dart:async、 和 dart:collection&mdash;在 Dart
  SDK 中已经包含了，文档位于 [api.dartlang.org]({{site.dart_api}})。
* Dart 社区分享的库通过库包的形式分享，
  发布在 [pub.dartlang.org](https://pub.dartlang.org/)。
  [pub](/tools/pub/) 工具可以用来创建、发布和管理库包。
* 来自于 GitHub 、任意 URL 地址或者本地路径的库也可以使用在你的应用中。
  详情请参考
  [Pub 依赖管理](/tools/pub/dependencies) 里面的
  [依赖源](/tools/pub/dependencies#dependency-sources)。
* 本地库位于你应用目录下的 `/lib` 目录，目录结构参考： [
应用目录结构](/tools/pub/package-layout#public-directories)。

本文档讨论前面两种库，
并告诉你在哪里学习其他常用的 Dart 库。

<aside class="alert alert-info" markdown="1">
**提示：**
如果你在 [api.dartlang.org]({{site.dart_api}}) 没有找到你需要的函数，
看看 [pub.dartlang.org](https://pub.dartlang.org/) 有没有。
</aside>

关于 web、 server、 或者 Flutter 的库请参考
[Specialized libraries](#specialized-libraries)。

## Dart SDK libraries

SDK 库（例如 dart:core, dart:async, dart:math, dart:convert）包含
一些 Dart 应用所需要的基础类。
其他非核心的类放到了 SDK 之外。

[核心库概览](/guides/libraries/library-tour) 介绍了 SDK 里面的
核心库的常用功能。

## Commonly used packages（常用的库）

Dart 社区的开发者已经开发了很多优秀的库了。
下面是一些非常流行和常用的库，
按照英文字母顺序排序：

| **Package** | **Description** | **Commonly used APIs** |
| [firebase](https://pub.dartlang.org/packages/firebase) | [Firebase](https://firebase.google.com) 扩展库，可以让你很简单的发布应用到云端。 | Auth, Database, Query, Storage |
| [http](https://pub.dartlang.org/packages/http) | 请求 HTTP 的扩展库。 | delete(), get(), post(), read() |
| [intl](https://pub.dartlang.org/packages/intl) | 强大的国际化和本地化工具库，支持很多格式化操作。 | Bidi, DateFormat, MicroMoney, TextDirection |
| [logging](https://pub.dartlang.org/packages/logging) | 强大的日志工具库。 | LoggerHandler, Level, LogRecord |
| [mockito](https://pub.dartlang.org/packages/mockito) | 测试过程中 mock 对象的库。如果你在为依赖注入编写测试代码，则这个库最适合你了。和 [test](https://pub.dartlang.org/packages/test) 库配合使用。 | Answering, Expectation, Verification |
| [path](https://pub.dartlang.org/packages/path) | 功能全面的跨平台的路径管理库。详情参考 [Unboxing Packages: path.](http://news.dartlang.org/2016/06/unboxing-packages-path.html) | absolute(), basename(), extension(), join(), normalize(), relative(), split() |
| [quiver](https://pub.dartlang.org/packages/quiver) | 扩展了 Dart 核心库，可以更简单的使用核心库和其他的库。对 async, cache, collection, core, iterables, patterns, 和 testing 提供了额外的支持。 | CountdownTimer (quiver.async); MapCache (quiver.cache); MultiMap, TreeSet (quiver.collection); EnumerateIterable (quiver.iterables); center(), compareIgnoreCase(), isWhiteSpace() (quiver.strings)  |
| [shelf](https://pub.dartlang.org/packages/shelf) | Dart Web 服务器的中间件。Shelf 让编写 web 服务器更加简单。 | Cascade, Pipeline, Request, Response, Server |
| [stack_trace](https://pub.dartlang.org/packages/stack_trace) | 一个异常堆栈分析工具，提供比 Dart 自带的 StackTrace 更加友好的方式来显示堆栈信息。还提供很多方便分析堆栈的工具，详情参考 [Unboxing Packages: stack_trace.](http://news.dartlang.org/2016/01/unboxing-packages-stacktrace.html) | Trace.current(), Trace.format(), Trace.from() |
| [stagehand](https://pub.dartlang.org/packages/stagehand) | Dart 项目生成器。 WebStorm 和 IntelliJ 使用 Stagehand 模板来创建新的项目，在命令行也可以使用该模板。 | Generally used through an IDE or the [`stagehand` command](http://stagehand.pub/). |
| [test](https://pub.dartlang.org/packages/test) | Dart 中编写和运行测试的标准方式。 | expect(), group(), test() |
{:.table .table-striped .nowrap}

了解更多各种库，请查看 [pub.dartlang.org](https://pub.dartlang.org/)。

## Packages that correspond to SDK libraries

下面这些扩展库都是建立在 SDK 核心库之上的，为核心库
提供了额外的功能和其他缺失的特性：

| **Package** | **Description** | **Commonly used APIs** |
| [async](https://www.dartdocs.org/documentation/async/latest/) | Expands on dart:async, adding utility classes to work with asynchronous computations. For more information, see [Unboxing Packages: async part 1](http://news.dartlang.org/2016/03/unboxing-packages-async-part-1.html), [part 2](http://news.dartlang.org/2016/03/unboxing-packages-async-part-2.html), and [part 3.](http://news.dartlang.org/2016/04/unboxing-packages-async-part-3.html) | AsyncMemoizer, CancelableOperation, FutureGroup, LazyStream, Result, StreamCompleter, StreamGroup, StreamSplitter |
| [collection](https://www.dartdocs.org/documentation/collection/latest) | Expands on dart:collection, adding utility functions and classes to make working with collections easier. For more information, see [Unboxing Packages: collection.](http://news.dartlang.org/2016/01/unboxing-packages-collection.html) | Equality, CanonicalizedMap, MapKeySet, MapValueSet, PriorityQueue, QueueList |
|[convert](https://www.dartdocs.org/documentation/convert/latest/) | Expands on dart:convert, adding encoders and decoders for converting between different data representations. One of the data representations is _percent encoding_, also known as _URL encoding_. | HexDecoder, PercentDecoder |
{:.table .table-striped .nowrap}

## Specialized libraries（专门的库）

一些更加专业的库在这里不做
介绍。

### Web libraries

如果你要编写 Web 应用，可以看看 Angular 2，这是一个 web 应用框架。
其他可选的资源是和 JS 互操作的 JavaScript API 以及
dart:html 库用来实现底层的 HTML 编程。

**详情：** [webdev.dartlang.org]({{site.webdev}})

### Dart VM libraries

如果你编写服务器或者命令行程序，参考
[dart:io](https://api.dartlang.org/stable/dart-io/dart-io-library.html)
和其他相关的库。

**详情：** [Dart VM]({{site.dart_vm}})

### Flutter libraries

如果你要编写移动应用，参考 Flutter。
Flutter SDK 的核心库文档位于 
[docs.flutter.io](http://docs.flutter.io/)。要使用这些苦，
 请按照文档 [Importing libraries from
packages](https://www.dartlang.org/tools/pub/get-started#importing-libraries-from-packages) 来操作。

**详情：** [flutter.io]({{site.flutter}})

## Resources

更多关于库和库包的信息请参考如下资源。

### Importing and using libraries

* [Using libraries](/guides/language/language-tour#libraries-and-visibility),
  a section in the [language tour](/guides/language/language-tour)
* [Importing library
  files](/guides/libraries/create-library-packages#importing-library-files),
  a section in [Create Library Packages](/guides/libraries/create-library-packages)
* [Pub docs](/tools/pub), particularly
  [Pub Package Layout Conventions](/tools/pub/package-layout) and
  [Dependency sources](/tools/pub/dependencies#dependency-sources)

### Creating library packages

* [创建库包](/guides/libraries/create-library-packages)

### Using specific libraries and packages

* [A Tour of the Dart Libraries](/guides/libraries/library-tour), which
  gives examples of many commonly used dart:* APIs.
* [Unboxing Packages](http://news.dartlang.org/search/label/Unboxing%20Packages)
  posts, written by written by Natalie Weizenbaum and published on
  [news.dartlang.org.](http://news.dartlang.org/).
  This page links to some of Natalie's posts, but she covers other packages
  not mentioned here, such as stream_channel, vm_service_client, and json_rpc_2.

### Packages contributed by the community

* [pub.dartlang.org](https://pub.dartlang.org)

### API reference documentation

* [api.dartlang.org]({{site.dart_api}}) contains the generated docs for dart:* libraries.
* [dartdocs.org](https://www.dartdocs.org/) contains the generated docs for
  packages published on pub.dartlang.org.
* [docs.flutter.io](http://docs.flutter.io/) contains the generated docs for Flutter
  libraries.

