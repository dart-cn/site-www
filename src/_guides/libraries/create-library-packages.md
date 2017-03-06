---
layout: default
title: "创建库包"
description: "学习如何创建 Dart 第三方库包。"
---

库是非常好的一种创建模块化代码的方式，并且也
易于分享代码。在 Dart 生态中，
库通过包的方式来创建和分发。
Dart 有两种类型的包：
[_应用_ 包](/tools/pub/glossary#application-package) 
和
[_库_ 包](/tools/pub/glossary#library-package)。

这篇文档介绍如何创建库包以及
其他参考文档链接。
关于_使用_库的详细信息，请参考
[安装第三方库](/tutorials/libraries/shared-pkgs) 或者
语言预览中关于
[库和可见性](/guides/language/language-tour#libraries-and-visibility)
部分。

## What makes a library package

下面显示了最简单的库包的
目录结构：

{% img 'libraries/simple-lib2.png' %}

对于一个最简单的库需要如下内容：

<dl markdown="1">

<dt markdown="1">
pubspec file
</dt>
<dd markdown="1">
库的 `pubspec.yaml` 文件和应用包的一样
&mdash;并没有特别的标志表明包是
一个库。
</dd>

<dt markdown="1">
lib directory
</dt>
<dd markdown="1">
库包的代码位于 _lib_ 目录中，
在其他包中可以访问这里面的代码。
在 lib 目录下可以创建任意的目录结构。
通常情况下都把实现代码放到 _lib/src_ 目录中。
位于 lib/src 下面的代码被认为是私有的；
其他包不应该直接导入 `src/...` 里面的代码。
要分享 lib/src 下的 API，你可以在 lib  目录下
创建一个文件，在这个文件中导入 lib/src 中的代码。
</dd>

<aside class="alert alert-info" markdown="1">
**注意**
当没有指定  `library`  指令的时候，根据每个库
的路径和文件名会为每个库生成一个唯一的标签。
所以，我们 建议你不要在代码中使用  `library`  指令，
除非你想
[生成库的 API 文档](#documenting-a-library)。
</aside>

</dl>

## Organizing a library package

当库包非常小的时候，是非常容易维护、扩展和测试的，
这样的库被称之为
_迷你库_。
在大部分情况下，每个类都应该位于一个迷你库中，
除非当两个类紧紧耦合在一起的时候需要放到同一个库中。

<aside class="alert alert-info" markdown="1">
**注意：** 你可能已经知道 `part` 指令了，该指令
可以把一个库分开到多个 Dart 文件中。我们推荐你
使用迷你库，而不要使用该指令。
</aside>

在 lib 目录下创建一个主要的库文件（
lib/_&lt;package-name&gt;_.dart），并且在该文件中
 导出所有公开的 API。
这样使用你的库的用户就可以只导入一个
文件就可以使用所有的功能了。

lib 目录还可以包含其他重要的非代码文件。例如，
你的库是跨平台的，你创建
了不同平台使用 dart:io 的库。
有些包有几个分开的库需要分别导入，
每个库都使用不同的前缀。

下面是 shelf 库的目录结构。
[shelf](https://github.com/dart-lang/shelf)
库提供了一种简单的方式来创建 web 服务器，
shelf 的目录结构是 Dart 库包目录结构
的常见形式：

{% img 'libraries/shelf.png' %}

在 lib 目录下有个主要的库文件 
`shelf.dart`，该文件导出了 lib/src 目录下的几个文件：

{% prettify dart %}
export 'src/cascade.dart';
export 'src/handler.dart';
export 'src/handlers/logger.dart';
export 'src/hijack_exception.dart';
export 'src/middleware.dart';
export 'src/pipeline.dart';
export 'src/request.dart';
export 'src/response.dart';
export 'src/server.dart';
export 'src/server_handler.dart';
{% endprettify %}

shelf 库还包含了一个迷你库： shelf_io。
这个适配器处理 dart:io 中的 HttpRequest 对象。

## Importing library files

可以使用 `package:` 指令并指定文件的 URI 来
导入库文件。

{% prettify dart %}
import 'package:utilities/utilities.dart';
{% endprettify %}

当两个文件都位于 lib 目录中
或者目录 lib 之外的时候，
可以使用相对路径来导入库文件。

然后，当导入的文件和使用的文件不在
同一个库的时候，需要使用 `package:` 指令。
当你不知道如何选择的时候，就使用 `package:` 指令。

下面显示了在 lib 和 web 库中如何导入
`lib/src/foo/a.dart` 。

{% img 'libraries/import-lib-rules.png' %}

## Providing additional files

良好的库应该很容易测试。
我们推荐你使用
[test](https://github.com/dart-lang/test) 库来编写测试代码，
把测试代码放到 `test` 目录
下。

如果你创建了一个可执行的工具，则可以把
这些工具放到 `bin` 目录，该目录是公开可访问的。
使用
[`pub global activate`](/tools/pub/cmd/pub-global#activating-a-package)
来启用命令行运行工具的功能。
在
[`executables` 部分](/tools/pub/pubspec#executables) 列出所有可执行文件，
可以让用户直接运行该命令，而不用通过
[`pub global run`](/tools/pub/cmd/pub-global#running-a-script-using-pub-global-run) 来执行。

在库中包含使用你的库的示例代码也是非常有用的，把这些代码
放到 `example` 目录下。

你在开发的时候创建的其他不应该共享的工具则可以
放到 `tool` 目录。

当你把库提交到 
[pub.dartlang.org](https://pub.dartlang.org) 的时候，需要包含 README 、 CHANGELOG 等文件，
在 [发布包](/tools/pub/publishing) 页面对这些文件有介绍。
同时还请参考 
[Pub 包约定目录结构](/tools/pub/package-layout)
来了解该如何布局包中的文件
目录。

## Documenting a library

使用 [dartdoc](https://github.com/dart-lang/dartdoc#dartdoc) 工具可以
为库生成 API 文档。
Dartdoc 从代码中解析
使用 `///` 语法的
[文档注释](/guides/language/effective-dart/documentation#doc-comments)。

{% prettify dart %}
/// The event handler responsible for updating the badge in the UI.
void updateBadge() {
  ...
}
{% endprettify %}

参考
[shelf 文档](https://www.dartdocs.org/documentation/shelf/latest/shelf/shelf-library.html) 来查看生成的 API 文档格式。

<aside class="alert alert-info" markdown="1">
**注意：**
要包含 库的文档需要使用  `library`
指令。
参考 [issue 1082](https://github.com/dart-lang/dartdoc/issues/1082) 了解详情。
</aside>

## Distributing a library

当把库项目提交到版本库中的时候，请注意有些
文件是不需要提交的。对于库
不要提交 `.packages`、 `pubspec.lock` 文件，
也不要提交 `packages`  目录。详情请参考：
[哪些文件不需要提交到版本库](private-files)。

{% comment %}
PENDING: here just to make it easy to find discussions of `packages`...
{% include packages-dir.html %}
{% endcomment %}

使用 [pub publish](/tools/pub/cmd/pub-lish) 工具可以在
[pub.dartlang.org](https://pub.dartlang.org/) 上
分享你的库。

[发布一个库](/tools/pub/publishing)
描述了一个库需要包含哪些文件。

[dartdocs.org 生成器](https://github.com/astashov/dartdocs.org)
提供了一些好用的服务供发布到 pub.dartlang.org 的开发者使用。
这些服务追踪库的版本发布，当有新版本发布的时候会自动
生成新的 API 文档，文档在
[dartdocs.org](https://www.dartdocs.org/) 可以查看。
在发布你的库之前，请先运行 dartdoc 看看生成
的文档是否满足你的要求。
如果你的库的文档在  dartdocs.org 中查找不到，请
查看
[dartdocs.org/failed](https://www.dartdocs.org/failed/index.html)
来了解原因。

为了避免版本发布引起的问题，
当引用 dartdocs.org 地址的时候，
 可以指定  "latest" 版本来替代具体的版本号。
例如：
[https://www.dartdocs.org/documentation/shelf/latest/](https://www.dartdocs.org/documentation/shelf/latest/).

## Resources

关于库包的更多资料请参考下面链接：

* [语言预览](/guides/language/language-tour) 中
  的[库和可见性](/guides/language/language-tour#libraries-and-visibility)
  介绍了如何使用库文件。
* [pub](/tools/pub) 文档非常有用，特别是
  [Pub 包约定目录结构](/tools/pub/package-layout)。
* [哪些文件不要提交到版本库](private-files)
  介绍了哪些文件不需要提交到代码库中。
* 在
  [dart-lang](https://github.com/dart-lang) 组织中的新包
  用来介绍创建包的最佳实践。推荐看看下面这些示例代码：
  [dart_style](https://github.com/dart-lang/dart_style),
  [path](https://github.com/dart-lang/path),
  [shelf](https://github.com/dart-lang/shelf),
  [source_gen](https://github.com/dart-lang/source_gen), 和
  [test](https://github.com/dart-lang/test)。
