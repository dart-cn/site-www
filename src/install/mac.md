---
layout: default
title: "在 Mac 安装 Dart"
description: "在 Mac 上使用 homebrew 来安装和更新 Dart SDK。"
permalink: /install/mac
---

Homebrew 是 Mac OS 的包管理器。
使用 [Homebrew](http://brew.sh/)
安装和升级 Dart 是非常简单的。

不想使用 homebrew？
你还可以选择 [手动安装](/install/archive)。

## 安装 {#homebrew-install-dart}

[Dart SDK](/tools/sdk)，
里面有 Dart VM、核心库、一些命令行工具，例如
[dart]({{site.dart_vm}}/tools)、
[dartanalyzer](https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli)、
[pub](/tools/pub)、
和 [dartdoc](https://github.com/dart-lang/dartdoc#dartdoc)。

[安装 homebrew](http://brew.sh/) 然后运行如下命令：

{% prettify shell %}
$ brew tap dart-lang/dart
$ brew install dart
{% endprettify %}

如果你需要开发 web 应用，则还需要安装 Dartium 和 Content Shell：

{% prettify shell %}
$ brew tap dart-lang/dart
$ brew install dart --with-content-shell --with-dartium
{% endprettify %}

### 安装开发版

使用 `--devel` 命令
来安装开发版：

{% prettify shell %}
$ brew install dart --devel
{% endprettify %}

下面这些参数可以随意组合：
`--devel`,
`--with-dartium`, 和
`--with-content-shell` 。

<aside class="alert alert-warning" markdown="1">
**警告：**
开发版用来体验最新的功能，
但是并没有像稳定版一样经过严格的测试，所以 会存在一些问题。
</aside>


## 更新 {#homebrew-update-dart}

使用 Homebrew 安装后，只需要运行如下命令即可更新 SDK：

{% prettify shell %}
$ brew update
$ brew upgrade dart
$ brew cleanup dart
{% endprettify %}

{% comment %}
PENDING: clarify what arguments you should supply,
depending on what arguments you used before.
{% endcomment %}


## 安装目录

像编辑器等工具都需要设置 Dart SDK 的目录以及
Dartium 的目录。
Homebrew 默认安装目录如下：
`HOMEBREW_INSTALL` 为你的 
homebrew 安装目录
(通过 `brew --prefix` 命令可以查看该目录)：

* SDK 目录: `HOMEBREW_INSTALL/opt/dart/libexec`
* Dartium: `HOMEBREW_INSTALL/opt/dart/Chromium.app`


### 自定义安装目录 {#homebrew-custom-location}

默认情况下，Homebrew 安装到 `/usr/local` 目录。
如果你的 Mac 设置安装 `/usr/local` 需要管理员
权限（使用`sudo`），我们推荐
安装到其他具有读写权限的目录中去，例如
你的 home 目录。

1. 导航到你想把 Homebrew 安装到那
   个目录下。Go to the directory above where you want
   例如，如果你想把 Homebrew 和 Dart 安装到 
   `~/homebrew`，则导航到 `~` 目录。

   {% prettify shell %}
   $ cd ~    # The directory that will contain Homebrew and Dart
   {% endprettify %}

2. Clone Homebrew。这样就在当前目录下创建了一个 `homebrew` 目录。

   {% prettify shell %}
   $ git clone https://github.com/Homebrew/homebrew.git
   {% endprettify %}

3. 把 `homebrew/bin` 目录添加到系统 PATH 中。

4. 按照 
[安装 Dart](#homebrew-install-dart) 中的方式来安装 Dart SDK。
Dart 会安装到`homebrew` 目录下。
