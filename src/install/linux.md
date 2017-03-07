---
layout: default
title: "在 Linux 安装 Dart"
description: "在 Linux 上使用 apt-get 、Debian 包管理器或者从源代码来安装和升级 Dart SDK。"
permalink: /install/linux

js:
- url: /install/archive/assets/install.js
  defer: true
---

如果你使用的 AMD64 (64-bit Intel) 的 Debian/Ubuntu，你有两种安装方式，
两种方式以后都可以
自动更新 SDK。

* [使用 apt-get 安装](#using-apt-get)
* [下载 Debian 包安装](#installing-a-debian-package)

还有另外两种安装方式：

* [手动安装](/install/archive)
* [从源代码自己编译](#compiling-from-source)

如果你做 web 开发，则还需要
<a data-bits="64" data-os="linux" data-tool="dartium"
    class="download-link"
    href="{{ site.custom.downloads.dartarchive-stable-url-prefix }}/latest/dartium/dartium-linux-x64-release.zip">按照 Dartium for Linux</a>.

## 使用 apt-get 安装 {#using-apt-get}

使用 apt-get 安装之前需要设置一些参数。

### 设置稳定版 {#setting-up-for-the-stable-channel}

下面只需执行一次的设置稳定版的命令。

{% prettify shell %}
# Enable HTTPS for apt.
$ sudo apt-get update
$ sudo apt-get install apt-transport-https
# Get the Google Linux package signing key.
$ sudo sh -c 'curl https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -'
# Set up the location of the stable repository.
$ sudo sh -c 'curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > /etc/apt/sources.list.d/dart_stable.list'
$ sudo apt-get update
{% endprettify %}


### 设置开发版

下面只需执行一次的设置开发版的命令。
请参考 [设置稳定版](#setting-up-for-the-stable-channel)。

{% prettify shell %}
# Before running this command, follow the instructions in
# "Set up for the stable channel".
$ sudo sh -c 'curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_unstable.list > /etc/apt/sources.list.d/dart_unstable.list'
{% endprettify %}


### 安装 SDK

下面的命令根据你前面的设置安装最高
版本的 SDK。

{% prettify shell %}
$ sudo apt-get install dart
{% endprettify %}

如果你同时设置了开发版和稳定版的设置，
由于开发版的版本号要比稳定版高，前面的命令只会
安装开发版。
如果你想安装稳定版或者安装
一个具体的版本，请参考下面的内容。


### 安装指定的版本

使用下面的命令可以
强迫安装稳定版。

{% prettify shell %}
$ sudo apt-get install dart/stable
{% endprettify %}

指定版本号可以安装指定的
版本，例如：

{% prettify shell %}
$ sudo apt-get install dart=1.5.8-1
$ sudo apt-get install dart=1.6.*
$ sudo apt-get install dart=1.7.0-dev.0.1.*
{% endprettify %}


## 安装 Debian package{#installing-a-debian-package}

使用下面的按钮来下载稳定版或者
开发版的 `.deb` 包。

{% include_relative _debian.html buttonclass="download-btn btn btn-primary btn-lg" %}

## 从源代码编译

你可以 [自己编译 SDK](https://github.com/dart-lang/sdk/wiki/Building) 。
