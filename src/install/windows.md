---
layout: default
title: "在 Windows 安装 Dart"
description: "使用 Chocolatey 或者 安装器在 Windows 安装和升级 Dart SDK。"
permalink: /install/windows

js:
- url: /install/archive/assets/install.js
  defer: true
---

不想用 Chocolatey 或者安装器？
也可以
[手动安装 Dart SDK](/install/archive)。

* [使用第三方安装器](#installer)
* [使用 Chocolatey 安装](#chocolatey)

## 使用第三方安装器 {#installer}

下载社区提供的
[Dart SDK Windows 安装器 ](http://www.gekorm.com/dart-windows/)。
和安装其他 Windows 软件一样，
在安装的时候可以选择稳定版
还是开发版的 SDK 和 Dartium。

## 使用 Chocolatey 安装 {#chocolatey}

使用 [Chocolatey](https://chocolatey.org/)
安装 Dart 也是非常简单的。

**dart** 安装包包含了 [Dart SDK](/tools/sdk)，
里面有 Dart VM、核心库、一些命令行工具，例如
[dart]({{site.dart_vm}}/tools)、
[dartanalyzer](https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli)、
[pub](/tools/pub)、
和 [dartdoc](https://github.com/dart-lang/dartdoc#dartdoc)。
如果你需要开发 web 应用，则还需要另外一个工具：

* [Dartium]({{site.webdev}}/tools/dartium):
  包含 Dart VM 的特殊版本的 Chromium 浏览器。
  可以用该浏览器直接运行 Dart 代码来调试你的
  web 应用，不用每次都编译为  JavaScript 来调试。

如果你在开发服务器程序，则只需要 `dart-sdk` 即可：

{% prettify shell %}
choco install dart-sdk -version <version>
choco install dartium  -version <version>
{% endprettify %}

当前稳定版本号为：
<span class="editor-build-rev-stable">[calculating]</span>.

{% comment %}
使用 `-dev<dev-version>` 可以安装开发版
的版本。例如：

{% prettify shell %}
choco install -y dart-sdk -version 1.12.0-dev.2.2
choco install -y dartium  -version 1.12.0-dev.2.2
{% endprettify %}
{% endcomment %}

关于 Chocolatey 对 Dart 的支持的更多信息，请参考：

* [Dart SDK package](https://chocolatey.org/packages/dart-sdk/)
  on [chocolatey.org](https://chocolatey.org/)
* [Dartium package](https://chocolatey.org/packages/dartium/)
  on [chocolatey.org](https://chocolatey.org/)
* [Blog post](http://divingintodart.blogspot.co.uk/2015/05/chocolatey-dart-packages-for-windows-110.html)
  on [Diving Into Dart](http://divingintodart.blogspot.co.uk/)
