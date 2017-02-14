---
layout: guide
title: "Effective Dart: API 设计"
description: "设计一致的可用的库。"

nextpage:
  url:
  title:
prevpage:
  url: /guides/language/effective-dart/usage
  title: "最佳实践"
---

下面的指南将指导你如何编写一致的可用的库 API。

* TOC
{:toc}

> 译者注：Dart 中的方法和函数（function 和 method）是不同的概念。函数是指一个类中的成员，也就是 Java 中的函数。而
> 方法是指非类中的函数。方法在 Dart 中也是继承至 Object，是一个对象。

## 命名

命名是编写易于阅读的、可维护代码的关键之一。
下面的最佳实践可以帮助你实现这个目标。

### **要** 使用一致的术语。

对于同样的东西要一直使用同样的名字。
如果在你的库之外已经存在一个广为人知的名字了，
请继续使用这个名字。

<div class="good">
{% prettify dart %}
pageCount         // 一个成员变量
updatePageCount() // 和 pageCount 名字一致。
toSomething()     // 和 Iterable 的 toList() 一致。
asSomething()     // 和 List 的 asMap() 一致。
Point             // 广为人知的概念。
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
renumberPages()      // 和 pageCount 不一致，导致混乱。
convertToSomething() // 和广为人知的 toX() 不一致。
wrappedAsSomething() // 和广为人知的 asX() 不一致。
Cartesian            // 对大多数用户来说都不知道笛卡尔坐标是啥。
{% endprettify %}
</div>

目标是尽量利用用户已知的内容。包括他们所熟知的领域、
核心库的习惯用法、
以及你的 API 的其他部分的使用习惯。在这些熟知的基础之上命名你的代码，
可以减少你的用户使用你的库的学习成本，
提高他们的生产效率。


### **避免** 缩写。

只使用广为人知的缩写，对于特有领域的缩写，请进来不要使用。
如果要使用，请 [正确的指定首字母大小写][caps]。

[caps]: /guides/language/effective-dart/style#identifiers

<div class="good">
{% prettify dart %}
pageCount
buildRectangles
IOStream
HttpRequest
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
numPages    // "num" is an abbreviation of number(of)
buildRects
InputOutputStream
HypertextTransferProtocolRequest
{% endprettify %}
</div>


### **推荐** 把最具描述性的名词放到最后。

最后一个单词应该可以描述所代表的东西。
你可以在之前添加其他前缀来进一步详细描述，例如 其他形容词。

<div class="good">
{% prettify dart %}
pageCount             // A count (of pages).
ConversionSink        // A sink for doing conversions.
ChunkedConversionSink // A ConversionSink that's chunked.
CssFontFaceRule       // A rule for font faces in CSS.
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
numPages                  // Not a collection of pages.
CanvasRenderingContext2D  // Not a "2D".
RuleFontFaceCss           // Not a CSS.
{% endprettify %}
</div>


### **考虑** 尽量让代码看起来像普通的句子。

当你不知道如何命名 API 的时候，尝试着用你的 API 写一些代码，
尽量让你写的代码看起来像普通的句子一样。

<div class="good">
{% prettify dart %}
// "If errors is empty..."
if (errors.isEmpty) ...

// "Hey, _subscription, cancel!"
_subscription.cancel();

// "Get the monsters where the monster has claws."
monsters.where((monster) => monster.hasClaws);
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
// Telling errors to empty itself, or asking if it is?
if (errors.empty) ...

// Toggle what? To what?
_subscription.toggle();

// Filter the monsters with claws *out* or include *only* those?
monsters.filter((monster) => monster.hasClaws);
{% endprettify %}
</div>

尝试着使用你自己的 API，并且阅读以下写出来的代码，可以帮助你提高命名的技能。
添加其他文学和语法修饰让代码看起来更像语法正确的句子
是不必要的。

<div class="bad">
{% prettify dart %}
if (theCollectionOfErrors.isEmpty) ...

monsters.producesANewSequenceWhereEach((monster) => monster.hasClaws);
{% endprettify %}
</div>


### **推荐** 使用名词短语来命名不是布尔类型的变量和属性。

读者关注属性是*什么*。如果用户更关心
*如何*确定一个属性，则很可能应该是一个函数，
并使用动词短语命名该函数。

<div class="good">
{% prettify dart %}
list.length
context.lineWidth
quest.rampagingSwampBeast
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
list.deleteItems
{% endprettify %}
</div>


### **推荐** 使用非命令式动词短语命名布尔类型的变量和属性。

布尔名称通常用在控制语句中当做条件，所以你需要让他在
控制条件中语感很好。比较下面的两个：

{% prettify dart %}
if (window.closeable) { ... }   // 形容词
if (window.canClose) { ... }    // 动词
{% endprettify %}

好的名字一般都使用如下类型的动词：

*   "to be" 形式： `isEnabled`, `wasShown`, `willFire`。这是
    最常见的的形式。

*   一个 [辅助动词][]： `hasElements`, `canClose`,
    `shouldConsume`, `mustSave`.

*   一个主动动词： `ignoresInput`, `wroteFile`. 由于经常引起歧义，所以这种形式比较少见。
    `loggedResult` 不是一个好名字，原因在于他有两种解释：
    "whether or not a result was logged" 或者 "the result that was logged"。
    同样， `closingConnection` 也可以是 "whether the connection is closing"
    或者 "the connection that is closing"。 *只有* 当名字可以预期的时候
    才使用主动动词。

[auxiliary verb]: https://en.wikipedia.org/wiki/Auxiliary_verb

可以使用命令式动词来区分布尔变量名字和函数名字。
一个布尔变量的名字不应该看起来像一个命令，告诉这个对象做什么事情。
原因在于访问一个变量的属性并没有修改对象的状态。
如果这个属性*确实*修改了对象的状态，则他应该
是一个函数。

<div class="good">
{% prettify dart %}
isEmpty
hasElements
canClose
closesWindow
canShowPopup
hasShownPopup
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
empty         // Adjective or verb?
withElements  // Sounds like it might hold elements.
closeable     // Sounds like an interface.
              // "canClose" reads better as a sentence.
closingWindow // Returns a bool or a window?
showPopup     // Sounds like it shows the popup.
{% endprettify %}
</div>


### **考虑** 省略命名布尔*参数*的动词。

提炼于上一条规则。对于命名布尔参数，没有动词的
名称通常看起来更加舒服。

<div class="good">
{% prettify dart %}
Isolate.spawn(entryPoint, message, paused: false)
new List.from(elements, growable: true)
new RegExp(pattern, caseSensitive: false)
{% endprettify %}
</div>


### **推荐** 使用命令式动词短语来命名带有副作用的函数或者方法。

函数通常返回一个结果给调用者，并且执行一些任务或者带有副作用。
在像 Dart 这种命令式语言中，调用函数通常为了实现其副作用：
可能改变了对象的内部状态、
产生一些输出内容、或者和外部世界沟通等。

这种类型的成员应该使用命令式动词短语来命名，强调
该成员所执行的任务。

<div class="good">
{% prettify dart %}
list.add()
queue.removeFirst()
window.refresh()
connection.downloadData()
{% endprettify %}
</div>

这样调用的代码看起来就像是要执行某个任务的命令。

### **考虑** 使用名词短语或者非命令式动词短语命名返回数据为主要功能的方法或者函数。

虽然这些函数可能也有副作用，但是其主要目的是返回一个数据给调用者。
如果该函数无需参数通常应该是一个 getter 。
有时候获取一个属性则需要一些参数，比如，
`elementAt()` 从集合中返回一个数据，但是需要一个
指定返回那个数据的参数。

在*语法*上看这是一个函数，其实*严格来说*其返回的是集合中的一个属性，
应该使用一个能够表示该函数返回的是*什么*的词语
来命名。

<div class="good">
{% prettify dart %}
list.elementAt(3)
string.codeUnitAt(4)
{% endprettify %}
</div>

这条规则比前一条要宽松一些。有时候一些
函数没有副作用，但仍然使用一个动词短语来命名，例如：
`list.take()` 或者 `string.split()`。


### **推荐** 使用 `to___()` 来命名把对象的状态转换到一个新的对象的函数。

一个转换函数返回一个新的对象，里面包含一些原对象的状态，
可能还有稍微的修改。
核心库中很多类似的函数命名为
`toXXX` 。

如果你也定义了一个转换函数，最好也使用同样的命名方式。

<div class="good">
{% prettify dart %}
list.toSet()
stackTrace.toString()
dateTime.toLocal()
{% endprettify %}
</div>


### **推荐** 使用 `as___()` 来命名把原来对象转换为另外一种表现形式的函数。

转换函数提供的是“快照功能”。返回的对象有自己的数据副本，修改原来对象的数据不会改变
返回的对象中的数据。另外一种函数返回的是同一份数据的另外一种
表现形式，返回的是一个新的对象，但是其内部引用的数据和原来对象引用的数据一样。
修改原来对象中的数据，新返回的对象中的数据也一起被修改。

这种函数在核心库中被命名为 `as___()`。

<div class="good">
{% prettify dart %}
list.asMap()
bytes.asFloat32List()
subscription.asFuture()
{% endprettify %}
</div>


### **避免** 在方法或者函数名称中描述参数。

在调用代码的时候可以看到参数，所以无需
再次显示参数了。

<div class="good">
{% prettify dart %}
list.add(element)
map.remove(key)
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
list.addElement(element)
map.removeKey(key)
{% endprettify %}
</div>

但是，对于具有多个类似的函数的时候，使用参数名字可以消除歧义，
这个时候应该带有参数名字。

<div class="good">
{% prettify dart %}
map.containsKey(key)
map.containsValue(value)
{% endprettify %}
</div>


## 库

下划线 ( `_` ) 表明这个成员只能在库内部访问，是库私有成员。
Dart 工具确保该规则生效。

### **推荐** 使用私有声明。

库中的公开声明&mdash;顶级定义或者在类中定义&mdash;是一种信号，
表示其他库可以并应该访问这些成员。
同时公开声明也是一种你的库需要实现的契约，当
使用这些成员的时候，应该实现其宣称的功能。

如果某个成员你不希望公开，则在成员名字之前添加一个 `_` 即可。
减少公开的接口让你的库更容易维护，也让用户更加容易掌握你的库如何使用。

另外，分析工具还可以分析出没有用到的私有成员定义，然后
告诉你可以删除这些无用的代码。
私有成员第三方代码无法调用而你自己在库中也没有使用，所以是无用的代码。


## 类型

Dart 支持很多内置的类型并且你还可以自定义自己的类型。当然
你也可以选择不使用类型。

### **避免** 定义使用简单的方法可以替代的只有一个成员的抽象类。

和 Java 不同的是， Dart 支持一等方法（first-class functions）、闭包和优雅的语法来使用它们。
如果你需要的只是一个回调函数，使用方法即可。
如果你定义了一个类，里面只有一个名字无意义的函数，
例如 `call` 或者 `invoke`，
这种情况最好用方法替代。

<div class="good">
{% prettify dart %}
typedef bool Predicate(item);
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
abstract class Predicate {
  bool test(item);
}
{% endprettify %}
</div>


### **避免** 定义只包含静态成员的类。

在 Java 和 C# 中，所有成员都必须定义到类中，所以
常常可以看到一些类只包含一些静态常量。
其他类使用这个类作为命名空间来使用里面的
静态成员。

Dart 具有顶级方法、变量和常量，所以你*无需*用一个类
来定义一些常量。如果你只需要一个命名空间，
则库是更好的一个选择。库支持导入前缀和显示/隐藏组合器（combinator）。
这些都是用来防止命名冲突的工具，
非常适合用来定义静态成员。

如果变量或者方法逻辑上不属于一个类，把它作为顶级成员定义即可。
如果你担心命名冲突，指定一个更加精确的名字或者
把这些代码移动到一个单独的库中 - 这样可以使用前缀导入这些成员。

<div class="good">
{% prettify dart %}
DateTime mostRecent(List<DateTime> dates) {
  return dates.reduce((a, b) => a.isAfter(b) ? a : b);
}

const _favoriteMammal = 'weasel';
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class DateUtils {
  static DateTime mostRecent(List<DateTime> dates) {
    return dates.reduce((a, b) => a.isAfter(b) ? a : b);
  }
}

class _Favorites {
  static const mammal = 'weasel';
}
{% endprettify %}
</div>

在 Dart 世界中， 类是用来定义*一种对象*的。从来
没有被初始化的对象通常意味着这个类是可以删除的。

然后，这条规则并不是强制的。对于一些常量或者枚举型的类型，
使用类来把相关的成员组织到一起可能也是合理的。当然，
使用库也是同样合理的。

<div class="good">
{% prettify dart %}
class Color {
  static const red = '#f00';
  static const green = '#0f0';
  static const blue = '#00f';
  static const black = '#000';
  static const white = '#fff';
}
{% endprettify %}
</div>


### **避免** 继承一个不打算被继承的类。

如果类的构造函数修改为一个工厂构造函数了，
该类的所有子类构造函数中调用这个构造函数的地方都会失败。
另外，如果一个类修改了其中调用了 `this` 的函数，也有可能
会导致子类中继承这些方法的方法无法正常工作，
调用顺序得不到保障。

上面这两点要求设计一个类的时候，就要考虑这个类是否是用来被子类化的。
可以使用文档注释来说明这种情况，也可以使用一个
显而易见的希望被继承的类名字，比如 `IterableBase`。
如果类的作者没有注明，你最好认为作者并不打算让你继承这个类。
否则的话，以后作者修改了这个类，可能会导致你的代码出问题。


### **要** 在文档中表明你的类是否打算被继承。

这条承接上一条规则，如果你的类希望被继承，
最好在文档中注明或者用 `Base` 作为
类名的后缀。


### **避免** 混入（mixin）一个不打算被混入的类。

如果一个类之前没有定义构造函数，后来添加了一个构造函数，则所有
之前混入到该类的代码都将不能正常使用。
这在类中通常是无害的修改，
但是原作者可能不知道你混入了这个类，
导致你的代码不能继续使用。

所以和继承一个类一样，这意味着你要用文档注明一个类是否可以当做 mixin 使用，
也可以使用 mixin 后缀来表示该类可以当做 mixin 使用，例如 `IterableMixin`。
如果既没有文档说明，类的后缀也不是 Mixin，则你最好不要
把这个类用作 mixin。


### **要** 在文档中注明你的类是否打算当做 mixin 使用。

在类文档中注明类是否可以当做 mixin 使用，还是只能当做 mixin 使用。
如果你的类打算只当做 mixin 使用，最好考虑使用
`Mixin` 作为类名字的结尾。


## 构造函数

Dart 构造函数和类的名字是一样的方法，
还可以添加其他标识符，这种被称之为
_命名构造函数_.

### **推荐** 使用构造函数而不是静态函数来创建对象。

构造函数使用 `new` 或者 `const` 调用，表明该调用的
主要目的是生成一个该类的实例，
或者其他实现了该接口的类的实例。

从来不 _必要_ 使用静态函数来创建实例。
命名构造函数让你可以清晰的指定对象是如何被创建的，
工厂构造函数可以让你构造子类或者对象的
实现类实例。

仍然有一些函数从本质上创建了一个新的对象但是使用的并不是构造函数风格。
例如 [`Uri.parse()`][uri.parse] 是一个静态函数，
从提供的参数中返回一个新的 URI 对象。同样，
实现了 [Builder pattern][] 模式的类使用静态函数更易于
阅读。

[uri.parse]: {{site.dart_api}}/dart-core/Uri/parse.html
[builder pattern]: http://en.wikipedia.org/wiki/Builder_pattern

但是，大部分情况下都应该使用构造函数。
当用户想要创建一个新的实例的时候，他们期望使用构造函数来
创建一个对象。

<div class="good">
{% prettify dart %}
class Point {
  num x, y;
  Point(this.x, this.y);
  Point.polar(num theta, num radius)
      : x = radius * math.cos(theta),
        y = radius * math.sin(theta);
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class Point {
  num x, y;
  Point(this.x, this.y);
  static Point polar(num theta, num radius) {
    return new Point(radius * math.cos(theta),
        radius * math.sin(theta));
  }
}
{% endprettify %}
</div>


### **考虑** 让构造函数为 `const` 的。

如果你的类的所有变量都是 final 的，构造函数只是初始化这些变量，
你可以把构造函数定义为 `const` 类型的。
这样用户可以把你的类用于需要常量的地方
&mdash;在其他大型常量内部、switch 语句、默认参数值 等
地方。

如果你不定义为 `const` 的，则无法在上面提到的这种情况使用你的对象。

注意：`const` 构造函数是公开 API 的契约，你一旦这样做了，就不要在以后修改为
非 `const` 的。如果你修改了，调用你的类的代码将无法工作。
如果你不想做这种保证，请不要声明构造函数为 `const` 的。
在实际项目中，`const` 构造函数对于简单的、
不可变的数据记录对象是很有用的。


## 成员

成员属于一个对象，可以是变量或者函数。

### **推荐** 把成员变量或者顶级变量定义为 `final` 类型。

不可变的状态 &mdash; 不随着时间改变 &mdash; 让开发者的工作更加简单。
类和库只包含最少的可变状态更
易于维护。

当然了，有时候使用可变的数据是非常有用的。但是，如果没必要，
你应该默认的把可以定义为 `final` 的变量和顶级变量定义
为 `final`。


### **要** 使用 getter 来定义访问属性的操作。

如果函数的名字带有 `get` 前缀，或者是一个像 `length` 或者 `size` 这样
的名称，这种情况通常最好定义该函数为一个 getter。
当全部满足下面的条件的时候，你应该使用一个 getter：

  * **没有参数。**
  * **返回一个值**
  * **没有副作用** 调用一个 getter 不应该改变对象外部可见的状态
  (内部缓存和延时初始化的状态可以发生变化)
  如果对象的状态在多次调用同一个 getter 之间没有发生变化，则
  多次调用同一个 getter 应该返回同一个值。

<div class="good">
{% prettify dart %}
rectangle.width
collection.isEmpty
button.canShow
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
DateTime.now;   // Returns different value each call.
window.refresh; // Doesn't return a value.
{% endprettify %}
</div>

和其他语言不通，Dart 并不要求 getter 执行的很快，或者具有一定的复杂度。
调用 Iterable 的 `length` 函数时间复杂度可能是
`O(n)`，这是可接受的。


### **要** 对于本质上为修改对象属性的函数要使用 setter。

如果函数的名字带有 `set`  前缀，通常意味着其应该是一个 setter。
具体来说，当满足以下条件的时候应该使用 setter：

*   **只有一个参数。**

*   **改变对象的某个状态。**

*   **有一个对应的 getter。** 对于用户可以修改但是无法查看的状态是比较奇怪的。
    (反之则是正常的，只有 getter 没有
    setter 是很常见的。)

*   **是幂等的。** 用同样的值多次调用同一个 setter， 后面的调用
    应该对对象没有不同的改变。

<div class="good">
{% prettify dart %}
rectangle.width = 3;
button.visible = false;
{% endprettify %}
</div>


### **不要** 定义没有对应 getter 的 setter 函数。

用户认为 getter 和 setter 是一个对象可见的属性。
一个可以写但是无法读的属性是比较奇怪的。
比如，只有 setter 没有 getter 意味着可以
使用 `=` 修改其值，但是使用 `+=` 则不行。

这条规则并不是告诉你需要添加一个 getter 只是因为你需要一个 setter。
对象应该只暴露他们需要的属性。
如果你有一些对象的状态可以修改但是无法获取，
则请使用函数而不要使用 setter。

<aside class="alert alert-info" markdown="1">

  There is one exception to this rule. An [Angular][] component class may expose
  setters that are invoked from a template to initialize the component. Often,
  these setters are not intended to be invoked from Dart code and don't need a
  corresponding getter. (If they are used from Dart code, they *should* have a
  getter.)

  [angular]: http://angular.io

</aside>

### **避免** 在返回值类型为 `bool`, `double`, `int`, 或者 `num` 的函数中返回 `null`。

尽管 Dart 中所有类型都可以为 null，用户所需要的数据通常是不包含  `null` 的，
另外这些名字为小写字符也隐含其意义为 Java 中的原始
类型类似，其值不为 null。

偶尔在你的 API 中使用 "nullable primitive" 类型也是有用的，
例如，代表在 map 中确实某个 key 的值，但是
这种情况比较少见。

如果你有一些函数返回值为 `null`，请在
文档中清晰的注明对应的解释，并且要包含在什么条件下会返回 `null`。


### **避免** 在函数中返回 `this` 只是为了串联调用函数。

对于这种情况， Dart 提供的函数级联调用是更好的选择。

<div class="good">
{% prettify dart %}
var buffer = new StringBuffer()
  ..write('one')
  ..write('two')
  ..write('three');
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
var buffer = new StringBuffer()
  .write('one')
  .write('two')
  .write('three');
{% endprettify %}
</div>


## 类型注解

在 Dart 中，为变量添加静态类型是可选的。

### **要** 在公开的 API 上指定类型。

类型注解是如何使用一个库的很重要的文档信息。
注解参数类型和返回值类型可以帮助
用户理解你的 API 所期望的参数以及所提供的功能。

注意，如果一个公开的 API 需要一些参数在 Dart 类型系统中没有定义，
则可以省略类型。这种情况下，隐含的
 `dynamic` *是* 该 API 的正确类型。

对于库内部使用的私有代码，你可以根据自己的
需要来选择是否使用类型，这里并不是必要的。

<div class="bad">
{% prettify dart %}
install(id, destination) {
  // ...
}
{% endprettify %}
</div>

上面的情况，对于 `id` 如何取值是不确定的。一个字符串？ `destination` 是什么意思呢？
一个字符串或者一个 `File` 对象？这个函数是异步的呢还是同步的？

<div class="good">
{% prettify dart %}
Future<bool> install(PackageId id, String destination) {
  // ...
}
{% endprettify %}
</div>

加上类型，这一切都清楚了。


### **不要** 为 setter 指定返回类型。

类型系统自动认为所有的 setter 返回的都是 `void`。

<div class="bad">
{% prettify dart %}
void set foo(Foo value) {...}
{% endprettify %}
</div>

<div class="good">
{% prettify dart %}
set foo(Foo value) {...}
{% endprettify %}
</div>


### **推荐** 为私有成员提供类型。

在公开的 API 上使用类型可以帮助使用你的库的用户。同样，
是私有代码上使用类型，可以帮助你的你的同事或者代码维护者。
另外，在私有成员上使用类型，对于将来自己查看代码
也有帮助。

<div class="good">
{% prettify dart %}
class CallChainVisitor {
  final SourceVisitor _visitor;
  final Expression _target;

  void _writeCall(Expression call) { ... }

  ...
}
{% endprettify %}
</div>


### **避免** 在方法表达式上使用类型。

方法表达式通常非接简洁。如果一个方法表达式复杂到需要
使用类型来表明其所做的功能，则应该使用一个方法或者函数来替代
这个表达式。相反，如果一个表达式足够简洁，通常
是不需要类型的。

<div class="good">
{% prettify dart %}
var names = people.map((person) => person.name);
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
var names = people.map((Person person) {
  return person.name;
});
{% endprettify %}
</div>


### **避免** 在没必要的地方使用 `dynamic` 类型。

在大部分 Dart 代码中，类型可以忽略，这样该参数类型会自动设置为 `dynamic`。
所以没必要手动指定类型为 `dynamic` 的，
只需要省略类型即可。

<div class="good">
{% prettify dart %}
lookUpOrDefault(String name, Map map, defaultValue) {
  var value = map[name];
  if (value != null) return value;
  return defaultValue;
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
dynamic lookUpOrDefault(String name, Map map, dynamic defaultValue) {
  var value = map[name];
  if (value != null) return value;
  return defaultValue;
}
{% endprettify %}
</div>


### **避免** 使用 `Function` 类型。

`Function` 类型通常比不使用类型更加精确。
如果你需要注解其类型，则最好使用一个能够描述其功能和返回值类型的方法
作为其类型，而不是定义个 `Function` 然后使用这个 `Function` 作为类型。

如果你在一个变量上使用注解，则意味着你需要创建一个 typedef，
但是大部分情况下这样做都是没有意义的。

<div class="good">
{% prettify dart %}
bool isValidString(String value, bool predicate(String string)) { ... }
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
bool isValidString(String value, Function predicate) { ... }
{% endprettify %}
</div>

有一种例外情况，如果变量可以取值为多种方法类型。
例如，他可能需要具有一个强制性的参数的方法或者
需要具有两个参数的方法。
由于在 Dart 中没有联合（union）类型，所以没法准确
的表示这种类型，通常只能使用 `dynamic`。 `Function` 会更加具体一点。


### **要** 使用 `Object` 来替代 `dynamic` 来表示可以接受任意对象。

有些操作函数可以使用任意对象作为参数。比如一个 log 函数
可以使用任意的对象并调用其 `toString()` 函数。
在 Dart 中有两种表示所有对象的类型：`Object` 和 `dynamic`。但是表达的意义不同。

`Object` 类型说明：可以接受任意对象，只需要这个对象定义了 `Object` 所定义的函数
即可。

而 `dynamic` 类型表达是意思是：没有一个类型可以表达你所期望的对象。
（有可能有类型可以表达，但是你
不在乎。）

<div class="good">
{% prettify dart %}
// Accepts any object.
void log(Object object) {
  print(object.toString());
}

// Only accepts bool or String, which can't be expressed in a type annotation.
bool convertToBool(arg) {
  if (arg is bool) return arg;
  if (arg is String) return arg == 'true';
  throw new ArgumentError('Cannot convert $arg to a bool.');
}
{% endprettify %}
</div>

## 参数

在 Dart 中可选参数可以为命名参数或者位置参数，但是不能同时有这两种类型的参数为可选参数。

### **避免** 位置参数作为可选布尔参数。

和其他类型不一样的是，布尔值通常使用字面量形式。
其他成员通常都放到一个命名的常量中，但是布尔值我们通常都直接使用
`true` 和 `false` 。如果起名不清晰的话，在使用布尔值调用的时候
代码看起来可能非常难懂：

<div class="bad">
{% prettify dart %}
new Task(true);
new Task(false);
new ListBox(false, true, true);
new Button(false);
{% endprettify %}
</div>

考虑使用命名参数或者命名构造函数以及命名常量来清晰
的表明您的意图：

<div class="good">
{% prettify dart %}
new Task.oneShot();
new Task.repeating();
new ListBox(scroll: true, showScrollbars: true);
new Button(ButtonState.enabled);
{% endprettify %}
</div>

注意，对于 setter 则没有这个要求，应为 setter 的名字已经明确的
表明了值所代表的意义。

<div class="good">
{% prettify dart %}
listBox.canScroll = true;
button.isEnabled = false;
{% endprettify %}
</div>

### **避免** 把用户想要忽略的参数放到位置可选参数的前列。

位置可选参数应该把经常使用的参数放到参数列表前面。
如果位置排列的不合理，则用户使用起来将很
麻烦。
对于拿不准的排序，请使用命名参数。

<div class="good">
{% prettify dart %}
String.fromCharCodes(Iterable<int> charCodes, [int start = 0, int end])

DateTime(int year,
    [int month = 1,
    int day = 1,
    int hour = 0,
    int minute = 0,
    int second = 0,
    int millisecond = 0,
    int microsecond = 0])

Duration(
    {int days: 0,
    int hours: 0,
    int minutes: 0,
    int seconds: 0,
    int milliseconds: 0,
    int microseconds: 0})
{% endprettify %}
</div>

### **避免** 使用强制无意义的参数。

如果用户可以省略一个参数调用函数，推荐让该参数为可选参数而不是强迫用户
使用 `null` 来作为参数。空字符串 等类似
的情况也适用这种情况。

省略参数看起来更加简洁，
有助于
防止 bug。

<div class="good">
{% prettify dart %}
string.substring(start)
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
string.substring(start, null)
{% endprettify %}
</div>


### **要** 使用包含开始位置并且不包含结束位置的范围参数。

如果你定义一个函数或者方法让用户从基于位置排序的集合中
选择一些元素，需要一个开始位置索引和结束位置索引分别制定开始
元素的位置以及结束元素的位置。结束位置通常是指
大于最后一个元素的位置的值。

核心库就是这样定义的，所以最好和核心库保持一致。

<div class="good">
{% prettify dart %}
[0, 1, 2, 3].sublist(1, 3) // [1, 2].
'abcd'.substring(1, 3)     // "bc".
{% endprettify %}
</div>

这种类型的参数保持一致是非常重要的，由于这种参数通常是位置参数，
如果你的函数第二个参数所代表的意义为获取元素的个数而不是结束的位置，
在调用的时候用户没法通过代码查看其区别。


## 相等判断

为类实现自定义的相等判断可能比较麻烦。关于两个对象是否相等，
用户有根深蒂固的直观感受，并且基于哈希的集合要求
里面的对象满足一些微妙
的协议。

### **要** 在覆写 `==` 的同时覆写 `hashCode` 。

默认的哈希函数实现了恒等式哈希&mdash;两个对象
只有当其是同一个对象的时候哈希值才一样。
否则的话，默认的 `==`  的行为不满足恒等式要求。

如果你覆写了 `==` ，则表明你的对象可能和其他对象相等。
**任何相等的两个对象都必须具有同样的哈希值。**
否则的话，map 和其他基于哈希的集合将不知道这两个对象是相等的。

### **要** 安装算术相等要规则来实现你的 `==` 操作符。

等价关系应该是这样的：

* **自反**: `a == a` 应该总是 `true`.

* **对称**: `a == b` 应该和 `b == a` 是一样的结果。

* **传递**: 如果 `a == b` 和 `b == c` 都返回 `true`，则 `a == c`
  也应该为 `true`。

用户以及使用 `==`  的代码都期望遵守上面的规则。
如果你的类无法满足这些要求，则 `==`  就不是你想
表达的函数的正确名字。

### **避免** 为可变对象自定义相等函数。

如果你定义了 `==` ，则你还应该定义 `hashCode` 函数。
这两个函数都应该考虑对象的变量。如果这些变量发生了*变化*，则
表明该对象的哈希值也应该变化。

大部分基于哈希的集合并不这样认为&mdash;这些集合
认为对象的哈希值应该一直不变，如果不是这样的话，这些集合
可能出现怪异的行为。

### **不要** 在自定义 `==` 操作符中判断 `null`。

语言规范表明了这种判断已经自动执行了，你的 `==` 自定义操作符只有当
右侧对象不为 null 的时候才会执行。

<div class="good">
{% prettify dart %}
class Person {
  final String name;

  operator ==(other) =>
      other is Person && name == other.name;
}
{% endprettify %}
</div>

<div class="bad">
{% prettify dart %}
class Person {
  final String name;

  operator ==(other) =>
      other != null &&
      other is Person &&
      name == other.name;
}
{% endprettify %}
</div>

