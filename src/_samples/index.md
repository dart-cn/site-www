---
title: "示例代码"
description: "Dart 习惯用户示例和到大型示例项目的链接。"
permalink: /samples
---

这里展示的示例并不完善&mdash;只是像想通过示例
学习 Dart 的人做了简单的介绍。
你还应该查看下面两个页面。

<div class="card-grid">
  <div class="card">
    <h3><a href="/guides/language/language-tour">语法预览</a></h3>
    <p>
      比这里更加详细的语法介绍以及各种示例代码。
    </p>
  </div>
  <!-- XXXXX TODO: XXXXX
  <div class="card">
    <h3><a href="/DOES_NOT_EXIST_YET">End-to-end repositories</a></h3>
    <p>For those who like to see projects in their entirety.</p>
  </div>
  -->
  <div class="card">
    <h3><a href="/dart-vm/dart-by-example">Cookbook</a></h3>
    <p>
      一些帮助你解决常见问题的
      示例代码片段。
    </p>
  </div>
</div>

## Hello World

喜闻乐见的 hello world:

{% prettify dart %}
void main() {
  print('Hello, World!');
}
{% endprettify %}

## Variables

这里没有显式的指定变量类型。也可以指定变量类型。

{% prettify dart %}
var name = 'Voyager I';
var year = 1977;
var antennaDiameter = 3.7;
var flybyObjects = ['Jupiter', 'Saturn', 'Uranus', 'Neptune'];
var image = {
  'tags': ['saturn'],
  'url': '//path/to/saturn.jpg'
};
{% endprettify %}

[更多关于](/guides/language/language-tour#variables) 变量的信息，包含默认值、 `final` 和 `const` 关键字等。

## Control flow statements

也是很常见的结构：

{% prettify dart %}
if (year >= 2001) {
  print('21st century');
} else if (year >= 1901) {
  print('20th century');
}

for (var object in flybyObjects) {
  print(object);
}

for (int month = 1; month <= 12; month++) {
  print(month);
}

while (year < 2016) {
  year += 1;
}
{% endprettify %}

[更多关于](/guides/language/language-tour#control-flow-statements) 控制流程语句的介绍，
包括 `break` 和 `continue`、 `switch` 和 `case`、以及 `assert`。

## Functions

如最佳实践所建议的，这里指定了方法参数类型和返回值类型。你有可以忽略这些类型。

{% prettify dart %}
int fibonacci(int n) {
  if (n == 0 || n == 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

var result = fibonacci(20);
{% endprettify %}

只有一个语句的方法可以使用缩写 (_胖箭头_) 语法来定义方法。
特别是当把方法作为函数参数来使用的时候，
这意味着，Hello World 示例可以变的更加[简短](https://gist.github.com/filiph/8a5e3e845acdafe2ea928fd257a46859)。

{% prettify dart %}
flybyObjects.where((name) => name.contains('anus')).forEach(print);
{% endprettify %}

注意上面示例中，顶级方法 `print` 被当做参数使用。

[更多关于](/guides/language/language-tour#functions) Dart 中方法的介绍，包括
optional parameters、 default parameter values、 lexical scope 等。

## Comments

{% prettify dart %}
// 普通的单行注释。

/// 文档注释。用来显示库、类以及
/// 他们的成员。IDEs 和工具使用文档注释。

/* 这种注释也是支持的。 */
{% endprettify %}

[更多关于](/guides/language/language-tour#comments) 注释的内容，
以及文档注释工具是如何使用的。

## Imports

{% prettify dart %}
// Importing core libraries
import 'dart:async';
import 'dart:math';

// Importing libraries from external packages
import 'package:angular2/angular2.dart';

// Importing files
import 'path/to/my_other_file.dart';
{% endprettify %}

[更多关于](/guides/language/language-tour#libraries-and-visibility) 库和可见性的介绍，
包括 library prefixes、 `show` 和 `hide`、 以及使用 `deferred`  关键字来延迟加载库。

## Classes

{% prettify dart %}
class Spacecraft {
  String name;
  DateTime launchDate;
  int launchYear;

  // Constructor, including syntactic sugar for assignment to members.
  Spacecraft(this.name, this.launchDate) {
    // Pretend the following is something you'd actually want to run in
    // a constructor.
    launchYear = launchDate?.year;
  }

  // Named constructor that forwards to the default one.
  Spacecraft.unlaunched(String name) : this(name, null);

  // Method.
  void describe() {
    print('Spacecraft: $name');
    if (launchDate != null) {
      int years = new DateTime.now().difference(launchDate).inDays ~/ 365;
      print('Launched: $launchYear ($years years ago)');
    } else {
      print('Unlaunched');
    }
  }
}
{% endprettify %}

可以这样使用上面定义的类：

{% prettify dart %}
var voyager = new Spacecraft('Voyager I', new DateTime(1977, 9, 5));
voyager.describe();

var voyager3 = new Spacecraft.unlaunched('Voyager III');
voyager3.describe();
{% endprettify %}

[更多关于](/guides/language/language-tour#classes) 类的介绍，
包括 initializer lists、 redirecting constructors、 constant constructors、 `factory` constructors、 getters、 setters、 等等。

## Inheritance

Dart 是单继承的。

{% prettify dart %}
class Orbiter extends Spacecraft {
  num altitude;
  Orbiter(String name, DateTime launchDate, this.altitude)
      : super(name, launchDate);
}
{% endprettify %}

[更多关于](/guides/language/language-tour#extending-a-class) 继承类的信息，还有 `@override` 注解等信息。

## Mixins

Mixins 是在多类继承体系中重用代码的一种方式。下面的类可以作为一个 mixin：

{% prettify dart %}
class Manned {
  int astronauts;
  void describeCrew() {
    print('Number of astronauts: $astronauts');
  }
}
{% endprettify %}

只需要扩展一个 mixin 类即可把 mixin 的功能添加到类中。

{% prettify dart %}
class Orbiter extends Spacecraft with Manned {
  // ...
}
{% endprettify %}

`Orbiter` 现在也有一个 `astronauts` 变量和 `describeCrew()` 函数。

[更多关于](/articles/language/mixins) mixins 的介绍。

## Interfaces

Dart 没有 `interface` 关键字。在 Dart 中所有的类都隐含的定义了一个接口。因此你可以使用 `implement` 来实现任意的类隐含定义的接口。

{% prettify dart %}
class MockSpaceship implements Spacecraft {
  // ...
}
{% endprettify %}

[更多关于](/guides/language/language-tour#implicit-interfaces) 接口的说明。

## Abstract classes

可以创建一个抽象类。抽象类可以包含抽象函数（没有函数体的函数）。

{% prettify dart %}
abstract class Describable {
  void describe();

  void describeWithEmphasis() {
    print('=========');
    describe();
    print('=========');
  }
}
{% endprettify %}

任何继承 `Describable` 的类都有一各 `describeWithEmphasis()` 函数，这个函数调用子类的 `describe()` 函数。

[更多关于](/guides/language/language-tour#abstract-classes) 抽象类和函数的介绍。

## Async

使用 `async` 和 `await` 可以避免回调接口嵌套的问题，让异步代码更加简洁。

{% prettify dart %}
Future<Null> printWithDelay(String message) async {
  await new Future.delayed(const Duration(seconds: 1));
  print(message);
}
{% endprettify %}

上面的代码和下面的代码是一样的：

{% prettify dart %}
Future<Null> printWithDelay(String message) {
  return new Future.delayed(const Duration(seconds: 1)).then((_) {
    print(message);
  });
}
{% endprettify %}

上面的代码，`async` 和 `await` 看起来并不太有用。
下面是示例显示了 `async` 和 `await`如何让异步代码变得
更加简单易读：

{% prettify dart %}
Future<Null> createDescriptions(Iterable<String> objects) async {
  for (var object in objects) {
    try {
      var file = new File('$object.txt');
      if (await file.exists()) {
        var modified = await file.lastModified();
        print('File for $object already exists. It was modified on $modified.');
        continue;
      }
      await file.create();
      await file.writeAsString('Start describing $object in this file.');
    } on IOException catch (e) {
      print('Cannot create description for $object: $e');
    }
  }
}
{% endprettify %}

你还可以使用 `async*`，可以使用优雅的方式来创建 stream：

{% prettify dart %}
Stream<String> report(Spacecraft craft, Iterable<String> objects) async* {
  for (var object in objects) {
    await new Future.delayed(const Duration(seconds: 1));
    yield '${craft.name} flies by $object';
  }
}
{% endprettify %}

[更多关于](/guides/language/language-tour#asynchrony-support) 异步的信息，包括 async functions、 `Future`、 `Stream`、 the asynchronous loop (`await for`)、等等。

## Exceptions

{% prettify dart %}
if (astronauts == 0) {
  throw new StateError('No astronauts.');
}
{% endprettify %}

异常可以被捕获，异步代码中也可以。

{% prettify dart %}
try {
  for (var object in flybyObjects) {
    var description = await new File('$object.txt').readAsString();
    print(description);
  }
} on IOException catch (e) {
  print('Could not describe object: $e');
} finally {
  flybyObjects.clear();
}
{% endprettify %}

[更多关于](/guides/language/language-tour#exceptions) 异常的信息，包括 Error 和 Exception 的区别、 stack traces、 `rethrow` 等等。

### Getters and setters

getter 和setter 是 Dart 中特殊的函数，看起来像是变量。可以像下面这样来重新定义 `Spacecraft` 类的 `launchYear` 属性：

{% prettify dart %}
class Spacecraft {
  // ...
  DateTime launchDate;
  int get launchYear => launchDate?.year;
  // ...
}
{% endprettify %}

还使用访问变量的方式 (`spacecraft.launchYear`) 来是会用这个属性。其实是调用的类的  getter 方法。

[更多关于](/guides/language/language-tour#getters-and-setters) getter 和 setter 的信息。

## Other topics

在 [语法预览](/guides/language/language-tour) 和 [核心库预览](/guides/libraries/library-tour) 中介绍了更多的示例代码。
库还在 [API 文档](https://api.dartlang.org/) 中提供了各种示例。
