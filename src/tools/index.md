---
layout: default
title: "Dart 工具"
description: "支持 Dart 语言开发的各种工具。"
permalink: /tools
toc: false
---

本页介绍一些有用的 Dart 工具。

## DartPad

[DartPad](/tools/dartpad) 是学习 Dart
语法和尝试 Dart 语言特性的最佳方式，还
可以体验 Dart 核心库（_除了_   dart:io 库和依赖 dart:io 的库）。

要编写使用其他库和具有更多功能的应用，
你需要一个 [SDK](#sdks)。
我们还推荐使用一个 IDE 来编写代码。


## IDEs

很多常见的 IDE 都有 Dart 的插件可以使用。
如果你还没有使用过 IDE，推荐你使用
 WebStorm，支持 Dart 开发。

<ul class="col2">
<li>
<img src="{% asset_path 'tools/IntellIJ-IDEA.png' %}" alt="IntelliJ logo">
<a href="/tools/jetbrains-plugin"><b>IntelliJ IDEA<br>
(and other JetBrains IDEs)</b></a>
</li>
<li>
<img src="{% asset_path 'tools/webstorm.png' %}" alt="WebStorm logo">
<a href="{{site.webdev}}/tools/webstorm"><b>WebStorm</b></a>
</li>
</ul>

下面是其他一些开源的 
Dart 插件：

<ul class="col2">
<li>
<img src="{% asset_path 'tools/atom-logo.png' %}" alt="Atom logo">
<a href="https://github.com/dart-atom/dartlang/"><b>Atom</b></a>
</li>
<li>
<img src="{% asset_path 'tools/emacs.png' %}" alt="Emacs logo">
<a href="https://github.com/nex3/dart-mode"><b>Emacs</b></a>
</li>
<li>
<img src="{% asset_path 'tools/sublime.png' %}" alt="Sublime logo">
<a href="https://github.com/dart-lang/dart-sublime-bundle#readme"><b>Sublime Text 3</b></a>
</li>
<li>
<img src="{% asset_path 'tools/vim.png' %}" alt="Vim logo">
<a href="https://github.com/dart-lang/dart-vim-plugin"><b>Vim</b></a>
</li>
<li>
<img src="{% asset_path 'tools/vscode.png' %}" alt="Visual Studio Code logo">
<a href="https://marketplace.visualstudio.com/items?itemName=DanTup.dart-code"><b>Visual Studio Code</b></a>
</li>
</ul>

## SDKs

开发不同类型的应用需要不同的 SDK。

|------------------------+----------+-------------------------------------|
| App type | SDK | Download instructions | More information |
|--------------------------|------------------------------------------------|
| Web app | Dart | [Install Dart and Dartium](/install) | [Dart SDK](/tools/sdk), [Dart Tools for the Web]({{site.webdev}}/tools) |
| Script or server | Dart | [Install Dart](/install) | [Dart SDK](/tools/sdk), [Dart VM Tools](/dart-vm/tools) |
| Mobile app | Flutter | [Flutter Setup]({{site.flutter}}/setup) | [flutter.io]({{site.flutter}}) |
{:.table .table-striped}



## Command-line tools

Dart SDK 包含下面这些工具：

[Pub package manager](/tools/pub)
: 管理 Dart 包，
  方便安装、使用和分享 Dart 库，
  命令行工具和其他资源。
  有些 Dart 框架，例如 Flutter， 可能只
  支持部分 pub 命令。
  支持 Dart 的 IDE 通常都支持 pub，
  如果 IDE 不支持，则可以使用命令行 (`pub`)。

[Static analyzer](https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli#dartanalyzer)
: 检测分析代码，潜在的问题会给出警告。
  IDE 插件通常都包含了 Dart 分析引擎，
  当然，也可以在命令行运行  (`dartanalyzer`)。

[代码格式化](https://github.com/dart-lang/dart_style#readme)
: 根据
  [Dart 风格指南](/guides/language/effective-dart/style)来格式化你的代码。
  IDE 通常也支持格式化 Dart 代码。
  也可以通过命令行运行 (`dartfmt`)。

关于这些命令的详细信息，请参考下面这些
资源：

* [Dart SDK](/tools/sdk) 和特殊的一些工具：
  * [Dart Tools for the Web]({{site.webdev}}/tools)
  * [Dart VM Tools]({{site.dart_vm}}/tools)
* [Flutter]({{site.flutter}}/setup/)
