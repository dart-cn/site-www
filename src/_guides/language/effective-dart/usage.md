---
layout: guide
title: "Effective Dart: 最佳实践"
description: "使用语言特性来编写可维护代码的指导手册。"

nextpage:
  url: /guides/language/effective-dart/design
  title: "设计"
prevpage:
  url: /guides/language/effective-dart/documentation
  title: "文档"
---

这部分是 Effective Dart 中最重要的内容。
在你的 Dart 代码中会一直使用这些指导原则。
使用你编写的库的*用户*可能不太注意到其中的问题，
但是*维护*你类库的人一定会发现其中的问题。

* TOC
{:toc}

## Strings

下面是 Dart 语言中和字符串相关的一些最佳实践。

### **要** 使用相邻的字符串字面量定义来链接字符串。

如果有两个字符串字面量定义&mdash;不是变量，而是实际的放到引号内的字符串
&mdash;你不用使用 `+` 来链接字符串。和 C 以及
C++ 一样，只要把他们放到一起即可。
 这种方式非常适合比较长的字符串定义，不能放到一行的情况。

<div class="good">
{% prettify dart %}
raiseAlarm(
    'ERROR: Parts of the spaceship are on fire. Other '
    'parts are overrun by martians. Unclear which are which.');
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
raiseAlarm(
    'ERROR: Parts of the spaceship are on fire. Other ' +
    'parts are overrun by martians. Unclear which are which.');
{% endprettify %}
</div>

### **推荐** 使用插值的形式来组合字符串和值。

如果你之前使用过其他语言，可能会遇到使用大量 `+` 来组合字符串的情况。
这种情况在 Dart 中也可以使用，但是使用字符串插值会让代码看起来
更加简洁和简短。

<div class="good">
{% prettify dart %}
'Hello, $name! You are ${year - birth} years old.';
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
'Hello, ' + name + '! You are ' + (year - birth) + ' years old.';
{% endprettify %}
</div>

### **避免** 在字符串插值中使用多余的大括号。

如果求值的只是一个简单的变量，并且后面没有紧跟随在其他字母文本，
则 `{}` 应该省略。

<div class="good">
{% prettify dart %}
'Hi, $name!'
"Wear your wildest $decade's outfit."
'Wear your wildest ${decade}s outfit.'
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
'Hi, ${name}!'
"Wear your wildest ${decade}'s outfit."
{% endprettify %}
</div>

## 集合

Dart 天生支持四种集合类型： lists、 maps、 queues、 和 sets。
下面的最佳实践是针对集合的。

### **要** 尽可能的使用集合字面量来定义集合。

有两种方式可以定义一个空的可变的 list：`[]` 和 `new List()`。
类似的，有三种方式可以定义一个空的 linked hash map： `{}`、 `new
Map()`、 和 `new LinkedHashMap()`。

如果你想创建一个不可变的 list，或者其他自定义类型的集合，你可以使用构造函数。
否则，使用优雅的字面量语法更加合理。
核心库中暴露这些构造函数易于扩展，但是通常在 Dart 代码
中并不使用构造函数。

<div class="good">
{% prettify dart %}
var points = [];
var addresses = {};
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
var points = new List();
var addresses = new Map();
{% endprettify %}
</div>

如果有必要还可以提供泛型类型。

<div class="good">
{% prettify dart %}
var points = <Point>[];
var addresses = <String, Address>{};
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
var points = new List<Point>();
var addresses = new Map<String, Address>();
{% endprettify %}
</div>

对于集合类的 *命名* 构造函数则不适用上面的规则。
`List.from()`、 `Map.fromIterable()` 都有其使用场景。 
如果需要一个固定长度的结合，
使用 `new List()` 来创建一个固定长度的 list 也是合理的。

### **不要** 使用 `.length` 来判断集合是否为空。

[Iterable][] 锲约并不要求集合知道其长度，也没要求
 在遍历的时候其长度不能改变。通过调用 `.length`  来判断
 集合是否包含内容是非常低效率的。

[iterable]: {{site.dart_api}}/dart-core/Iterable-class.html

相反，Dart 提供了更加高效率和易用的 getter 函数：
`.isEmpty` 和`.isNotEmpty`。使用这些函数并不需要对结果再次取非。

<div class="good">
{% prettify dart %}
if (lunchBox.isEmpty) return 'so hungry...';
if (words.isNotEmpty) return words.join(' ');
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
if (lunchBox.length == 0) return 'so hungry...';
if (!words.isEmpty) return words.join(' ');
{% endprettify %}
</div>

### **考虑** 使用高阶（higher-order）函数来转换集合数据。

如果你有一个集合并且想要修改里面的内容转换为另外一个集合，
使用 `.map()`、 `.where()` 以及 `Iterable` 提供的其他函数会
让代码更加简洁。

使用这些函数替代 `for` 循环会让代码更加可以表述你的意图，
生成一个新的集合系列并不具有副作用。

<div class="good">
{% prettify dart %}
var aquaticNames = animals
    .where((animal) => animal.isAquatic)
    .map((animal) => animal.name);
{% endprettify %}
</div>

如果你串联或者嵌套调用很多高阶函数，则使用
一些命令式代码可能会
更加清晰。

### **避免** 在 `Iterable.forEach()` 中使用函数声明形式。

`forEach()` 方法通常在 JavaScript 中使用，原因是系统内置的 `for-in` 
 循环并不能提供期望的结果。
 相反，在 Dart 中如果需要遍历一个集合，通常使用循环语句。

<div class="good">
{% prettify dart %}
for (var person in people) {
  ...
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
people.forEach((person) {
  ...
});
{% endprettify %}
</div>

如果你只想在每个集合元素上调用一个已经定义好的函数，则可以使用
`forEach()` 函数。

<div class="good">
{% prettify dart %}
people.forEach(print);
{% endprettify %}
</div>

## 方法(Functions)

在 Dart 中，方法都是对象。下面是关于调用方法的
一些最佳实践。

### **要** 用方法声明的形式来给方法起个名字。

现代的编程语言都意识到局部嵌套方法以及闭包是非常有用的。
通常是在一个方法中定义另外一个方法。在大部分情况下，
这些嵌套的方法都用作回调函数并且不需要名字。
一个方法表达式非常擅长这种情况。

但是，如果你确实需要给方法一个名字，请使用方法定义而不是把
lambda 赋值给一个变量。

<div class="good">
{% prettify dart %}
void main() {
  localFunction() {
    ...
  }
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
void main() {
  var localFunction = () {
    ...
  };
}
{% endprettify %}
</div>

### **不要** 使用 lambda 表达式来替代 tear-off。

如果你在一个对象上调用函数并省略了括号， Dart 称之为
 "tear-off"&mdash;一个和函数使用同样参数的闭包，当你调用他的时候就执行
 这个函数。

如果你有一个方法使用该方法同样的参数调用一个函数，
你无需手工的把该函数调用包装为一个 lambda 表达式。

<div class="good">
{% prettify dart %}
names.forEach(print);
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
names.forEach((name) {
  print(name);
});
{% endprettify %}
</div>

## 变量

下面的最佳实践是关于如何在 Dart 中使用变量的。

### **不要** 显式的把变量初始化为 `null`。

在 Dart 中没有初始化的变量和域会自动的
初始化为 `null`。在语言基本就保证了该行为的可靠性。
在 Dart 中没有 “未初始化的内存”这个概念。所以添加 
`= null` 是多余的。

<div class="good">
{% prettify dart %}
int _nextId;

class LazyId {
  int _id;

  int get id {
    if (_nextId == null) _nextId = 0;
    if (_id == null) _id = _nextId++;

    return _id;
  }
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
int _nextId = null;

class LazyId {
  int _id = null;

  int get id {
    if (_nextId == null) _nextId = 0;
    if (_id == null) _id = _nextId++;

    return _id;
  }
}
{% endprettify %}
</div>


### **避免** 保存可以计算的结果。

在设计类的时候，你常常希望暴露底层状态的多个表现属性。
常常你会发现在类的构造函数中计算这些属性，然后保存
起来：

<div class="bad">
{% prettify dart %}
class Circle {
  num radius;
  num area;
  num circumference;

  Circle(num radius)
      : radius = radius,
        area = math.PI * radius * radius,
        circumference = math.PI * 2.0 * radius;
}
{% endprettify %}
</div>

上面的代码有两个不妥之处。首先，浪费了内存。
严格来说 面积和周长 是*缓存*对象。他们保存的结果
可以通过已知的数据计算出来。他们主要用来减少 CPU 消耗而增加了内存消耗。
我们是否知道这里有一个需要权衡的性能问题？

更坏的情况是，上面的代码是 *错的*。上面的缓存是 *无效的*&mdash;你如何
知道何时缓存失效了需要重新计算？这里我们无从得知，
但是 `radius`  确是可变的。你可以给 `radius`  设置一个不同的值，但是
 `area` 和 `circumference` 还是之前的值。

为了避免缓存失效，我们需要这样做：

<div class="bad">
{% prettify dart %}
class Circle {
  num _radius;
  num get radius => _radius;
  set radius(num value) {
    _radius = value;
    _recalculate();
  }

  num _area;
  num get area => _area;

  num _circumference;
  num get circumference => _circumference;

  Circle(this._radius) {
    _recalculate();
  }

  void _recalculate() {
    _area = math.PI * _radius * _radius,
    _circumference = math.PI * 2.0 * _radius;
  }
}
{% endprettify %}
</div>

这需要编写、维护、调试以及阅读更多的代码。
如果你一开始这样写代码：

<div class="good">
{% prettify dart %}
class Circle {
  num radius;

  num get area => math.PI * radius * radius;
  num get circumference => math.PI * 2.0 * radius;

  Circle(this.radius);
}
{% endprettify %}
</div>

上面的代码更加简洁、使用更少的内存、减少出错的可能性。只是
保存了尽可能少的数据，这样无需更新缓存，因为就没有缓存，面积和周长
是通过计算得来的。

在某些情况下，当计算结果比较费时的时候可能需要缓存，但是只有当你发现
这样引起性能问题的时候才去缓存它，并且仔细的考虑实现方式并留下
对应的注释来解释你所做的优化。


### **考虑** 省略局部变量的类型。

现代的代码趋势是保持函数体尽可能的短，而局部变量的类型
通常都可以通过初始化语句推算出来，所以
显式的定义局部变量类型通常都是制造视觉噪音。

Dart 具有强大的静态分析工具，可以推断出局部变量的
 类型并且仍然可以提供代码自动补全以及你所期望的工具支持。

<div class="good">
{% prettify dart %}
Map<int, List<Person>> groupByZip(Iterable<Person> people) {
  var peopleByZip = <int, List<Person>>{};
  for (var person in people) {
    peopleByZip.putIfAbsent(person.zip, () => <Person>[]);
    peopleByZip[person.zip].add(person);
  }
  return peopleByZip;
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
Map<int, List<Person>> groupByZip(Iterable<Person> people) {
  Map<int, List<Person>> peopleByZip = <int, List<Person>>{};
  for (Person person in people) {
    peopleByZip.putIfAbsent(person.zip, () => <Person>[]);
    peopleByZip[person.zip].add(person);
  }
  return peopleByZip;
}
{% endprettify %}
</div>


## 成员

在 Dart 中， 对象的成员可以是 方法（函数）或者 数据（实例变量）。
下面的最佳实践是关于对象成员的。

### **不要** 创建没必要的 getter 和 setter。

在Java 和 C# 中通常为了隐藏成员变量而使用一个空的
  getter 和 setter 函数。
 如果你通过 getter 访
 问成员变量和
 直接访问成员
 变量是不一样的。

Dart 语言没有这种区别。 成员变量和 getter/setter 是完全一样的。
你可以一开始暴露一个成员变量，以后再使用 getter 和 setter 来修改
其相关的逻辑，而调用你类的代码不用做任何修改。

<div class="good">
{% prettify dart %}
class Box {
  var contents;
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class Box {
  var _contents;
  get contents => _contents;
  set contents(value) {
    _contents = value;
  }
}
{% endprettify %}
</div>


### **推荐** 使用 `final` 关键字来限定只读属性。

如果你有个变量其他人只能读取，
而不能修改其值，最简单的做法就是使用 `final` 关键字来标记这个变量。

<div class="good">
{% prettify dart %}
class Box {
  final contents = [];
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class Box {
  var _contents;
  get contents => _contents;
}
{% endprettify %}
</div>

当然了，如果你确实需要在构造函数以为内部赋值变量的值，
你可以需要这种“私有成员变量，公开访问函数”的模式，
但是，如非必要，请不要使用这种模式。


### **考虑** 用 `=>` 来实现只有一个单一返回语句的函数。

除了可以使用 `=>` 作为方法表达式以外， Dart 也允许使用其
 定义成员函数。对于简单的计算并返回的情况非常
 合适。

<div class="good">
{% prettify dart %}
get width => right - left;
bool ready(num time) => minTime == null || minTime <= time;
containsValue(String value) => getValues().contains(value);
{% endprettify %}
</div>

虽然多行代码也可以使用 `=>`，但是为了
表述的简洁，对于多行代码还是尽量使用
普通的花括号函数体并使用明显的 `return` 语句。

对于 `void` 类型的成员则 *不是* 一种期望的使用场景。
读者期望 `=>` 返回一个有用的值，所以对于没有返回值的情况，还是使用
`{ ... }` 使代码更加清晰。


### **不要** 使用 `this.` ，除非遇到了变量冲突的情况。

JavaScript 需要使用 `this.` 来引用对象的成员变量，但是
Dart&mdash;和 C++, Java, 以及
C#&mdash;没有这种限制。

只有当局部变量和成员变量名字一样的时候，你才需要使用 `this.`
 来访问成员变量。

<div class="bad">
{% prettify dart %}
class Box {
  var value;

  void clear() {
    this.update(null);
  }

  void update(value) {
    this.value = value;
  }
}
{% endprettify %}
</div>

<div class="good">
{% prettify dart %}
class Box {
  var value;

  void clear() {
    update(null);
  }

  void update(value) {
    this.value = value;
  }
}
{% endprettify %}
</div>

注意：构造函数参数在初始化参数列表中从来
不会出现参数冲突的情况。

<div class="good">
{% prettify dart %}
class Box extends BaseBox {
  var value;

  Box(value)
      : value = value,
        super(value)
      {}
}
{% endprettify %}
</div>

上面的代码看起来有点奇怪，但是其是按照你期望的方式工作的。
幸运的是，由于初始化规则的特殊性，上面的代码很少见到。


### **要** 尽可能的在定义变量的时候初始化其值。

如果一个变量不依赖于构造函数中的参数，则应该在定义
变量的时候就初始化其值。这样可以减少需要的代码并可以确保
在有多个构造函数的时候你不会忘记初始化该变量。

<div class="bad">
{% prettify dart %}
class Folder {
  final String name;
  final List<Document> contents;

  Folder(this.name) : contents = [];
  Folder.temp() : name = 'temporary'; // Oops! Forgot contents.
}
{% endprettify %}
</div>

<div class="good">
{% prettify dart %}
class Folder {
  final String name;
  final List<Document> contents = [];

  Folder(this.name);
  Folder.temp() : name = 'temporary';
}
{% endprettify %}
</div>

当然，对于变量取值依赖构造函数参数的情况以及
不同的构造函数取值也不一样的情况，则不适合本条规则。


## 构造函数

下面的最佳实践应用于类的构造函数

### **要** 尽可能的使用初始化形式。

很多变量都直接使用构造函数参数来初始化，例如：

<div class="bad">
{% prettify dart %}
class Point {
  num x, y;
  Point(num x, num y) {
    this.x = x;
    this.y = y;
  }
}
{% endprettify %}
</div>

为了初始化一个值，我们需要写四次 `x` 。我们可以做的更优雅：

<div class="good">
{% prettify dart %}
class Point {
  num x, y;
  Point(this.x, this.y);
}
{% endprettify %}
</div>

这里的位于构造函数参数之前的 `this.` 语法被称之为
初始化形式（initializing formal）。
有些情况下这无法使用这种形式。特别是，这种形式下无法在
初始化列表中看到变量。 如果能使用该方式就尽量
使用吧。


### **不要** 在初始化形式上定义类型。

如果构造函数使用 `this.` 来初始化成员变量，则参数的类型
一定是和变量的类型是一样的。

<div class="good">
{% prettify dart %}
class Point {
  int x, y;
  Point(this.x, this.y);
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class Point {
  int x, y;
  Point(int this.x, int this.y);
}
{% endprettify %}
</div>


### **要** 用 `;` 来替代空函数体的构造函数 `{}`。

在 Dart 中，没有具体函数体的构造函数可以使用分号结尾。
（事实上，这是不可变构造函数的要求。）

<div class="good">
{% prettify dart %}
class Point {
  int x, y;
  Point(this.x, this.y);
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class Point {
  int x, y;
  Point(this.x, this.y) {}
}
{% endprettify %}
</div>


### **要** 把 `super()` 调用放到构造函数初始化列表之后调用。

成员变量初始化是按照他们出现在构造函数初始化列表的顺序来初始化的。
如果你把 `super()` 调用放到初始化列表中间，则
超类的变量初始化会在之类初始化完成之前
调用。

但是这并不意味着超类的构造函数体就会执行。
不管你在何处调用`super()` ，
超类的构造函数只有在所有成员初始化完成后才会执行。
把 `super()`  放到其他地方则只有让代码看起来比较费解。
实际上，[DDC][] *要求*其出现在最后。 

[ddc]: https://github.com/dart-lang/dev_compiler

<div class="good">
{% prettify dart %}
View(Style style, List children)
    : _children = children,
      super(style) {
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
View(Style style, List children)
    : super(style),
      _children = children {
{% endprettify %}
</div>


## 错误处理

Dart 使用异常表示程序出现了错误。
下面的最佳实践是关于如何捕获和抛出异常的。

### **避免** 使用没有 `on` 语句的 catch。

一个没有 `on` 限定的 catch 语句会捕获 try catch 快内的 *任何* 异常。
 [Pokémon exception handling][pokemon] 不是你所希望的行为。
你的代码是否正确的处理 [StackOverflowError][] 或者
[OutOfMemoryError][] 异常了？如果你使用错误的参数调用函数，
你是期望调试器定位出你的错误使用情况还是
把这个有用的 [ArgumentError][] 给吞噬了？
由于你捕获了 [AssertionError][] 异常，导致
所有 try 块内的 `assert()` 语句都失效了，这是你需要的结果吗？

答案和可能是 "no"，这种情况下你需要过滤捕获的列表。
大部分情况下你都需要使用 `on` 来限定捕获的具体异常
类型。

在极少数情况下，你可能希望捕获所有运行时异常。这通常用在框架中
或者底层的代码中尝试隔离应用的代码
来避免产生问题。即使如何，通常 catch [Exception][] 比 catch 所有异常要好。
Exception 是所有 *运行时* 异常的基类，而不包含
可能是代码中编码的 bug 的异常。

[pokemon]: https://blog.codinghorror.com/new-programming-jargon/
[StackOverflowError]: {{site.dart_api}}/dart-core/StackOverflowError-class.html
[OutOfMemoryError]: {{site.dart_api}}/dart-core/OutOfMemoryError-class.html
[ArgumentError]: {{site.dart_api}}/dart-core/ArgumentError-class.html
[AssertionError]: {{site.dart_api}}/dart-core/AssertionError-class.html
[Exception]: {{site.dart_api}}/dart-core/Exception-class.html


### **不要** 丢弃没有使用 `on` 语句捕获的异常。

如果你真的期望捕获一段代码内的 *所有* 异常，请
*在捕获异常的地方做些事情*。 记录下来并显示给用户，或者
重新抛出（rethrow）异常信息，记得不要默默的丢弃该异常信息。


### **要** 只在代表编程错误的情况下才抛出实现了 `Error` 的异常。

[Error][] 类是所有 *编码* 错误的基类。当一个该类型或者其子
类型 例如 [ArgumentError][] 对象被抛出了，这意味着是
你代码中的一个 *bug*。当你的 API 想要告诉调用者使用错误的时候
 可以抛出一个 Error 来表明你的意图。

同样的，如果一个异常表示为运行时异常而不是代码 bug， 则抛出 
Error 则会误导调用者。
应该抛出核心定义的 Exception 类
或者其他类型。


### **不要** 显示的捕获 `Error` 或者其子类。

本条衔接上一天内容。既然 Error 表示代码中的 bug，则需要修复
该问题而不是
捕获该问题。

[error]: {{site.dart_api}}/Error-class.html

捕获这类错误打破了处理流程并且代码中有 bug。
不要在这里使用错误处理代码，而是需要到
导致该错误出现的地方修复你的代码。


### **要** 使用 `rethrow` 来重新抛出捕获的异常。

如果你想重新抛出一个异常，推荐使用 `rethrow` 语句。
`rethrow` 保留了原来的异常堆栈信息。 
而 `throw` 会把异常堆栈信息
重置为最后抛出的位置。

<div class="bad">
{% prettify dart %}
try {
  somethingRisky();
} catch(e) {
  if (!canHandle(e)) throw e;
  handle(e);
}
{% endprettify %}
</div>

<div class="good">
{% prettify dart %}
try {
  somethingRisky();
} catch(e) {
  if (!canHandle(e)) rethrow;
  handle(e);
}
{% endprettify %}
</div>


## 异步

Dart 具有几个语言特性来支持异步编程。
下面的最佳实践是针对异步编程的。

### **推荐** 使用 async/await 而不是直接使用底层的特性。

显式的异步代码是非常难以阅读和调试的，即使使用
很好的抽象（比如 future）也是如此。这就是为何 Dart
提供了 `async`/`await`。这样可以显著的提高代码的可读性并且让你可以在
异步代码中使用
语言提供的所有流程控制语句。

<div class="good">
{% prettify dart %}
Future<bool> doAsyncComputation() async {
  try {
    var result = await longRunningCalculation();
    return verifyResult(result.summary);
  } catch(e) {
    log.error(e);
    return false;
  }
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
Future<bool> doAsyncComputation() {
  return longRunningCalculation().then((result) {
    return verifyResult(result.summary);
  }).catchError((e) {
    log.error(e);
    return new Future.value(false);
  });
}
{% endprettify %}
</div>

### **不要** 在没有有用效果的情况下使用 `async` 。

当成为习惯之后，你可能会在所有和异步相关的
函数使用 `async`。但是在有些情况下，
如果可以忽略 `async`  而不改变方法的行为，则应该这么做：

<div class="good">
{% prettify dart %}
Future afterTwoThings(Future first, second) {
  return Future.wait([first, second]);
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
Future afterTwoThings(Future first, second) async {
  return Future.wait([first, second]);
}
{% endprettify %}
</div>

下面这些情况 `async` 是有用的：

* 你使用了 `await`。 (这是一个很明显的例子。)

* 你在异步的抛出一个异常。 `async` 然后 `throw` 比
  `return new Future.error(...)` 要简短很多。

* 你在返回一个值，但是你希望他显式的使用 Future。
  `async` 比 `new Future.value(...)` 要简短很多。

* 你不希望在事件循环发生事件之前执行
   任何代码。

<div class="good">
{% prettify dart %}
Future usesAwait(Future later) async {
  print(await later);
}

Future asyncError() async {
  throw 'Error!';
}

Future asyncValue() async {
  return 'value';
}
{% endprettify %}
</div>

### **考虑** 使用高阶函数来转换事件流（stream）

This parallels the above suggestion on iterables. Streams support many of the
same methods and also handle things like transmitting errors, closing, etc.
correctly.

### **避免** 直接使用 Completer  。

很多异步编程的新手想要编写生成一个 future 的代码。
而 Future 的构造函数看起来并不满足他们的要求，然后他们就
发现 Completer 类并使用它：

<div class="bad">
{% prettify dart %}
Future<bool> fileContainsBear(String path) {
  var completer = new Completer<bool>();

  new File(path).readAsString().then((contents) {
    completer.complete(contents.contains('bear'));
  });

  return completer.future;
}
{% endprettify %}
</div>

Completer 是用于两种底层代码的：
新的异步原子操作和集成没有使用 Future 的异步代码。
大部分的代码都应该使用 async/await 或者 [`Future.then()`][then]，
 这样代码更加清晰并且异常处理更加容易。

[then]: {{site.dart_api}}/dart-async/Future/then.html

<div class="good">
{% prettify dart %}
Future<bool> fileContainsBear(String path) {
  return new File(path).readAsString().then((contents) {
    return contents.contains('bear');
  });
}
{% endprettify %}
</div>

<div class="good">
{% prettify dart %}
Future<bool> fileContainsBear(String path) async {
  var contents = await new File(path).readAsString();
  return contents.contains('bear');
}
{% endprettify %}
</div>

