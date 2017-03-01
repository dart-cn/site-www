---
layout: guide
title: "Dart 库预览"
description: "了解 Dart 核心库中的主要特性和功能。"
short-title: Dart 库预览
---

这里讲介绍 Dart 核心库中的主要特性和功能。
只是一个预览，并不会面面俱到。
如果你想了解更详细的信息，请参考
[Dart API 文档]({{site.dart_api}})。

关于 Dart 语言的更多信息请参考：
[Dart 语言预览](/guides/language/language-tour)。

## dart:core - numbers, collections, strings, and more

Dart core 库提供了少量的但是非常核心的功能。每个 Dart 
应用都会自动导入这个
库。


### Numbers

dart:core 库定义了 num, int, 和 double 类，这些类
定义一些操作数字的基础功能。

使用 `parse()` 函数可以把字符串
转换为 整数或者浮点数。

<!-- library-tour/number-tests/bin/main.dart -->
{% prettify dart %}
assert(int.parse('42') == 42);
assert(int.parse('0x42') == 66);
assert(double.parse('0.50') == 0.5);
{% endprettify %}

num 类也定义了一个 parse() 函数，这个函数会尝试解析
为整数，如果无法解析为整数，则会解析为浮点数(double)。

<!-- library-tour/number-tests/bin/main.dart -->
{% prettify dart %}
assert(num.parse('42') is int);
assert(num.parse('0x42') is int);
assert(num.parse('0.50') is double);
{% endprettify %}

对于整数，可以指定一个 `radix` 作为基数：

<!-- library-tour/number-tests/bin/main.dart -->
{% prettify dart %}
assert(int.parse('42', radix: 16) == 66);
{% endprettify %}

使用 `toString()` 函数 (由
[Object]({{site.dart_api}}/dart-core/Object-class.html) 对象定义)来把整数或者浮点数
转换为字符串。使用 num 类的函数  `toStringAsFixed()`  可以限定
小数点的位数。如果要指定转换为字符串的有效位数，
则可以使用定义在 num 类的
函数  `toStringAsPrecision()`：

<!-- library-tour/number-tests/bin/main.dart -->
{% prettify dart %}
// Convert an int to a string.
assert(42.toString() == '42');

// Convert a double to a string.
assert(123.456.toString() == '123.456');

// Specify the number of digits after the decimal.
assert(123.456.toStringAsFixed(2) == '123.46');

// Specify the number of significant figures.
assert(123.456.toStringAsPrecision(2) == '1.2e+2');
assert(double.parse('1.2e+2') == 120.0);
{% endprettify %}

更多信息，参考 API 文档中每个类的描述：
[int,]({{site.dart_api}}/dart-core/int-class.html)
[double,]({{site.dart_api}}/dart-core/double-class.html) 和
[num。]({{site.dart_api}}/dart-core/num-class.html) 另外还有
[dart:math](#dartmath---math-and-random)提供了关于数学运算的常用功能。


### Strings and regular expressions（字符串和正则表达式）

Dart 中的字符串是一个不可变的 UTF-16 码元（code units）序列。
在语言预览中对
[字符串](/guides/language/language-tour#strings)有详细的介绍。
你可以使用正则表达式（RegExp 对象）
来搜索字符串中的内容或者
替换部分字符串。

String 类定义了一些函数 `split()`, `contains()`,
`startsWith()`, `endsWith()` 等来处理各种字符串操作。

#### Searching inside a string（在字符串内搜索）

可以在字符串内查找特定字符的位置，还可以
判断字符串是否以特定字符串开始和结尾。
例如：

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
// Check whether a string contains another string.
assert('Never odd or even'.contains('odd'));

// Does a string start with another string?
assert('Never odd or even'.startsWith('Never'));

// Does a string end with another string?
assert('Never odd or even'.endsWith('even'));

// Find the location of a string inside a string.
assert('Never odd or even'.indexOf('odd') == 6);
{% endprettify %}

#### Extracting data from a string（从字符串中提取数据）

可以从字符串中获取到单个的字符，单个字符可以是 String 也可以是 int 值。
准确来说，实际得到的是一个 UTF-16 code units；
对于码率比较大的字符，实际得到的是两个 code units，例如
treble clef 符号 ('\\u{1D11E}') 。

还可以从字符串中提取一个子字符串，或者把字符串分割为
多个子字符串：

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
// Grab a substring.
assert('Never odd or even'.substring(6, 9) == 'odd');

// Split a string using a string pattern.
var parts = 'structured web apps'.split(' ');
assert(parts.length == 3);
assert(parts[0] == 'structured');

// Get a UTF-16 code unit (as a string) by index.
assert('Never odd or even'[0] == 'N');

// Use split() with an empty string parameter to get
// a list of all characters (as Strings); good for
// iterating.
for (var char in 'hello'.split('')) {
  print(char);
}

// Get all the UTF-16 code units in the string.
var codeUnitList = 'Never odd or even'.codeUnits.toList();
assert(codeUnitList[0] == 78);
{% endprettify %}

#### Converting to uppercase or lowercase（大小写转换）

字符串大小写转换是非常
简单的：

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
// Convert to uppercase.
assert('structured web apps'.toUpperCase() ==
    'STRUCTURED WEB APPS');

// Convert to lowercase.
assert('STRUCTURED WEB APPS'.toLowerCase() ==
    'structured web apps');
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
注意上面的转换方式对于某些语言是有问题的。例如对于土耳其语言
 中的无点 *I* 的转换就是错误的。
</div>


#### Trimming and empty strings（裁剪和判断空字符串）

`trim()` 函数可以删除字符串前后的空白字符。使用 `isEmpty` 可以
判断字符串是否为空（长度为 0）。

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
// Trim a string.
assert('  hello  '.trim() == 'hello');

// Check whether a string is empty.
assert(''.isEmpty);

// Strings with only white space are not empty.
assert(!'  '.isEmpty);
{% endprettify %}

#### Replacing part of a string（替换部分字符）

Strings 是不可变的对象，可以创建他们但是无法修改。
如果你仔细研究了 [String API
文档]({{site.dart_api}}/dart-core/String-class.html)，你会注意到并没有
 函数可以修改字符串的状态。
例如，函数 `replaceAll()` 返回一个新的 String 对象而不是修改
旧的对象：

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
var greetingTemplate = 'Hello, NAME!';
var greeting = greetingTemplate
    .replaceAll(new RegExp('NAME'), 'Bob');

assert(greeting !=
    greetingTemplate); // greetingTemplate didn't change.
{% endprettify %}

#### Building a string（创建字符串）

使用 StringBuffer 可以在代码中创建字符串。
只有当你调用 StringBuffer 的 `toString()` 函数的时候，才会创建一个
新的 String 对象。而 `writeAll()` 函数还有一个可选的参数来指定每个字符串的分隔符，
例如下面指定空格为分隔符：

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
var sb = new StringBuffer();
sb..write('Use a StringBuffer for ')
  ..writeAll(['efficient', 'string', 'creation'], ' ')
  ..write('.');

var fullString = sb.toString();

assert(fullString ==
    'Use a StringBuffer for efficient string creation.');
{% endprettify %}

#### Regular expressions（正则表达式）

RegExp 类提供了 JavaScript 正则表达式同样的功能。
正则表达式可以高效率的搜索和匹配
字符串。

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
// Here's a regular expression for one or more digits.
var numbers = new RegExp(r'\d+');

var allCharacters = 'llamas live fifteen to twenty years';
var someDigits = 'llamas live 15 to 20 years';

// contains() can use a regular expression.
assert(!allCharacters.contains(numbers));
assert(someDigits.contains(numbers));

// Replace every match with another string.
var exedOut = someDigits.replaceAll(numbers, 'XX');
assert(exedOut == 'llamas live XX to XX years');
{% endprettify %}

还可以直接操作 RegExp 类。 Match 类提供了
访问正则表达式匹配到的内容。

<!-- library-tour/string-tests/bin/main.dart -->
{% prettify dart %}
var numbers = new RegExp(r'\d+');
var someDigits = 'llamas live 15 to 20 years';

// Check whether the reg exp has a match in a string.
assert(numbers.hasMatch(someDigits));

// Loop through all matches.
for (var match in numbers.allMatches(someDigits)) {
  print(match.group(0)); // 15, then 20
}
{% endprettify %}

#### More information（更多信息）

参考 [String API
文档]({{site.dart_api}}/dart-core/String-class.html) 来查看 String 类的所有
方法。同时还可以参考下面这些类的 api 文档：
[StringBuffer,]({{site.dart_api}}/dart-core/StringBuffer-class.html)
[Pattern,]({{site.dart_api}}/dart-core/Pattern-class.html)
[RegExp,]({{site.dart_api}}/dart-core/RegExp-class.html) 和
[Match.]({{site.dart_api}}/dart-core/Match-class.html)

### Collections

Dart 提供了一些核心的集合 API，包含
lists, sets, 和 maps。

#### Lists

在语言预览中已经介绍过如何
创建 [lists](#lists)了。另外还可以
使用 List 构造函数来创建 List 对象。
List 类来定义了一些函数可以添加或者删除里面的数据。

<!-- library-tour/list-tests/bin/main.dart -->
{% prettify dart %}
// Use a List constructor.
var vegetables = new List();

// Or simply use a list literal.
var fruits = ['apples', 'oranges'];

// Add to a list.
fruits.add('kiwis');

// Add multiple items to a list.
fruits.addAll(['grapes', 'bananas']);

// Get the list length.
assert(fruits.length == 5);

// Remove a single item.
var appleIndex = fruits.indexOf('apples');
fruits.removeAt(appleIndex);
assert(fruits.length == 4);

// Remove all elements from a list.
fruits.clear();
assert(fruits.length == 0);
{% endprettify %}

使用 `indexOf()` 来查找 list 中对象的索引：

<!-- library-tour/list-tests/bin/main.dart -->
{% prettify dart %}
var fruits = ['apples', 'oranges'];

// Access a list item by index.
assert(fruits[0] == 'apples');

// Find an item in a list.
assert(fruits.indexOf('apples') == 0);
{% endprettify %}

排序一个 list 可以使用 `sort()` 函数。还可以提供一个用来排序
的比较方法。排序方法返回值
为：对于*小的值* 为 \< 0；对于*相同*的值为 0 ；对于*大的值*为 \> 0。
下面的示例使用由 
 [Comparable]({{site.dart_api}}/dart-core/Comparable-class.html)  定义的 `compareTo()`
 函数，该函数也被 String 实现了。

<!-- library-tour/list-tests/bin/main.dart -->
{% prettify dart %}
var fruits = ['bananas', 'apples', 'oranges'];

// Sort a list.
fruits.sort((a, b) => a.compareTo(b));
assert(fruits[0] == 'apples');
{% endprettify %}

List 是泛型类型，所以可以指定
里面所保存的数据类型：

<!-- library-tour/list-tests/bin/main.dart -->
{% prettify dart %}
// This list should contain only strings.
var fruits = new List<String>();

fruits.add('apples');
var fruit = fruits[0];
assert(fruit is String);

// Generates static analysis warning, num is not a string.
fruits.add(5);  // BAD: Throws exception in checked mode.
{% endprettify %}

参考 [List API
文档]({{site.dart_api}}/dart-core/List-class.html) 来查看 list 的
所有函数。

#### Sets

Dart 中的 Set 是一个无序集合，里面不能保护重复的数据。
由于是无序的，所以无法通过索引来从 set 中获取数据：

<!-- library-tour/set-tests/bin/main.dart -->
{% prettify dart %}
var ingredients = new Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);
assert(ingredients.length == 3);

// Adding a duplicate item has no effect.
ingredients.add('gold');
assert(ingredients.length == 3);

// Remove an item from a set.
ingredients.remove('gold');
assert(ingredients.length == 2);
{% endprettify %}

使用 `contains()` 和 `containsAll()` 来判断 set 中是否包含
一个或者多个对象：

<!-- library-tour/set-tests/bin/main.dart -->
{% prettify dart %}
var ingredients = new Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// Check whether an item is in the set.
assert(ingredients.contains('titanium'));

// Check whether all the items are in the set.
assert(ingredients.containsAll(['titanium', 'xenon']));
{% endprettify %}

交际是两个 set 中都有的数据的子集：

<!-- library-tour/set-tests/bin/main.dart -->
{% prettify dart %}
var ingredients = new Set();
ingredients.addAll(['gold', 'titanium', 'xenon']);

// Create the intersection of two sets.
var nobleGases = new Set.from(['xenon', 'argon']);
var intersection = ingredients.intersection(nobleGases);
assert(intersection.length == 1);
assert(intersection.contains('xenon'));
{% endprettify %}

详细信息，
参考 [Set API 文档]({{site.dart_api}}/dart-core/Set-class.html)。

#### Maps

map 通常也被称之为 *字典*或者 *hash* ，也是一个无序的集合，里面
包含一个 key-value 对。map 把 key 和 value 关联起来可以
方便获取数据。和 JavaScript 不同的是， Dart objects 不是 maps。

下面是两种定义 map 对象的方式：

<!-- library-tour/map-1/bin/main.dart -->
{% prettify dart %}
// Maps often use strings as keys.
var hawaiianBeaches = {
  'Oahu'      : ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai'     : ['Hanalei', 'Poipu']
};

// Maps can be built from a constructor.
var searchTerms = new Map();

// Maps are parameterized types; you can specify what
// types the key and value should be.
var nobleGases = new Map<int, String>();
{% endprettify %}

使用中括号来访问或者设置 map 中的数据，
使用 `remove()` 函数来从 map 中删除 key 和 value。

<!-- library-tour/map-1/bin/main.dart -->
{% prettify dart %}
var nobleGases = {54: 'xenon'};

// Retrieve a value with a key.
assert(nobleGases[54] == 'xenon');

// Check whether a map contains a key.
assert(nobleGases.containsKey(54));

// Remove a key and its value.
nobleGases.remove(54);
assert(!nobleGases.containsKey(54));
{% endprettify %}

还可以获取 map 的所有 key 和 value：

<!-- library-tour/map-1/bin/main.dart -->
{% prettify dart %}
var hawaiianBeaches = {
  'Oahu'      : ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai'     : ['Hanalei', 'Poipu']
};

// Get all the keys as an unordered collection
// (an Iterable).
var keys = hawaiianBeaches.keys;

assert(keys.length == 3);
assert(new Set.from(keys).contains('Oahu'));

// Get all the values as an unordered collection
// (an Iterable of Lists).
var values = hawaiianBeaches.values;
assert(values.length == 3);
assert(values.any((v) => v.contains('Waikiki')));
{% endprettify %}

`containsKey()` 判断 map 是否包含一个 key。由于 map 的 value 可以为 null，
所有通过 key 来获取 value 并通过 判断 value 是否为 null 来判断 key 是否存在是
行不通的。所以添加了一个判断 key 是否存在的函数：

<!-- library-tour/map-1/bin/main.dart -->
{% prettify dart %}
var hawaiianBeaches = {
  'Oahu'      : ['Waikiki', 'Kailua', 'Waimanalo'],
  'Big Island': ['Wailea Bay', 'Pololu Beach'],
  'Kauai'     : ['Hanalei', 'Poipu']
};

assert(hawaiianBeaches.containsKey('Oahu'));
assert(!hawaiianBeaches.containsKey('Florida'));
{% endprettify %}

map 还有一个 `putIfAbsent()` 函数来设置 key 的值，但是只有该 key 
在 map 中不存在的时候才设置这个值，否则 key 的值保持不变。该函数需要
一个方法返回 value：

<!-- library-tour/map-1/bin/main.dart -->
{% prettify dart %}
var teamAssignments = {};
teamAssignments.putIfAbsent(
    'Catcher', () => pickToughestKid());
assert(teamAssignments['Catcher'] != null);
{% endprettify %}

详细信息请参考
 [Map API 文档]({{site.dart_api}}/dart-core/Map-class.html)。

#### Common collection methods（常用的集合函数）

List, Set, 和 Map 上可以使用很多常用的集合函数。
Iterable 类定义了一些常用的功能，
List 和 Set 实现了 Iterable 。

<div class="alert alert-info" markdown="1">
**注意：**
虽然 Map 没有实现 Iterable，但是 Map 的
 `keys` 和 `values` 属性实现了 Iterable。
</div>

可以使用 `isEmpty` 函数来判断集合是否为空的：

<!-- library-tour/collection-is-empty/bin/main.dart -->
{% prettify dart %}
var teas = ['green', 'black', 'chamomile', 'earl grey'];
assert(!teas.isEmpty);
{% endprettify %}

使用 `forEach()` 函数可以对集合中的每个数据都应用
一个方法：

<!-- library-tour/collection-apply-function/bin/main.dart -->
{% prettify dart %}
var teas = ['green', 'black', 'chamomile', 'earl grey'];

teas.forEach((tea) => print('I drink $tea'));
{% endprettify %}

在 Map 上使用 `forEach()` 的时候，方法需要能
接收两个参数（key 和 value）：

<!-- library-tour/map-1/bin/main.dart -->
{% prettify dart %}
hawaiianBeaches.forEach((k, v) {
  print('I want to visit $k and swim at $v');
  // I want to visit Oahu and swim at
  // [Waikiki, Kailua, Waimanalo], etc.
});
{% endprettify %}

Iterables 也有一个 `map()` 
函数，这个函数返回一个包含所有数据的对象：

<!-- library-tour/collection-apply-function/bin/main.dart -->
{% prettify dart %}
var teas = ['green', 'black', 'chamomile', 'earl grey'];

var loudTeas = teas.map((tea) => tea.toUpperCase());
loudTeas.forEach(print);
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
`map()` 函数返回的对象也是一个 Iterable，该对象是*懒求值（lazily 
evaluated）* 的，只有当访问里面的值的时候，
map 的方法才被调用。
</div>

可以使用 `map().toList()` 或者  `map().toSet()` 来
强制立刻执行 map 的方法：

<!-- library-tour/collection-apply-function/bin/main.dart -->
{% prettify dart %}
var loudTeaList = teas
    .map((tea) => tea.toUpperCase())
    .toList();
{% endprettify %}

Iterable 的 `where()` 函数可以返回所有满足特定条件的数据。
`any()` 判断是否有数据满足特定条件， `every()` 判断是否所有数据都满足
特定条件。
{% comment %}
PENDING: Change example as suggested by floitsch to have (maybe)
cities instead of isDecaffeinated.
{% endcomment %}


<!-- library-tour/collection-any-every/bin/main.dart -->
{% prettify dart %}
var teas = ['green', 'black', 'chamomile', 'earl grey'];

// Chamomile is not caffeinated.
bool isDecaffeinated(String teaName) =>
    teaName == 'chamomile';

// Use where() to find only the items that return true
// from the provided function.
var decaffeinatedTeas = teas
    .where((tea) => isDecaffeinated(tea));
// or teas.where(isDecaffeinated)

// Use any() to check whether at least one item in the
// collection satisfies a condition.
assert(teas.any(isDecaffeinated));

// Use every() to check whether all the items in a
// collection satisfy a condition.
assert(!teas.every(isDecaffeinated));
{% endprettify %}

更多信息请参考 [Iterable API
文档]({{site.dart_api}}/dart-core/Iterable-class.html)，以及 
List, Set, 和 Map 的文档。


### URIs

[Uri 类]({{site.dart_api}}/dart-core/Uri-class.html) 提供了
编码和解码 URI（URL） 字符的功能。 
这些函数处理 URI 特殊的字符，例如 `&` 和 `=`。
Uri 类还可以解析和处理 URI 的每个部分，比如
host, port, scheme 等。
{% comment %}
{PENDING: show
constructors: Uri.http, Uri.https, Uri.file, per floitsch's suggestion}
{% endcomment %}

#### Encoding and decoding fully qualified URIs（编码解码URI）

要编码和解码*除了* URI 中特殊意义（例如 `/`, `:`, `&`, `#`）的字符，
则可以使用 `encodeFull()` 和
`decodeFull()` 函数。这两个函数可以用来编码和解码整个
URI，并且保留 URI 特殊意义的字符不变。

<!-- library-tour/encode-uri/bin/main.dart -->
{% prettify dart %}
var uri = 'http://example.org/api?foo=some message';

var encoded = Uri.encodeFull(uri);
assert(encoded ==
    'http://example.org/api?foo=some%20message');

var decoded = Uri.decodeFull(encoded);
assert(uri == decoded);
{% endprettify %}

注意上面 `some` 和 `message` 之间的空格被编码了。  

#### Encoding and decoding URI components（编码解码URI组件）

使用 `encodeComponent()` 和 `decodeComponent()` 可以编码
和解码 URI 中的所有字符，特殊意义的字符（`/`, `&`, 和 `:` 等）
也会编码，

<!-- library-tour/encode-uri-components/bin/main.dart -->
{% prettify dart %}
var uri = 'http://example.org/api?foo=some message';

var encoded = Uri.encodeComponent(uri);
assert(encoded ==
    'http%3A%2F%2Fexample.org%2Fapi%3Ffoo%3Dsome%20message');

var decoded = Uri.decodeComponent(encoded);
assert(uri == decoded);
{% endprettify %}

注意上面特殊字符也被编码了，比如 `/`
编码为 `%2F`。

#### Parsing URIs

如果有个 Uri 对象或者 URI 字符串，使用 Uri 的属性
可以获取每个部分，比如 `path`。使用 `parse()` 静态
函数可以从字符串中解析一个 Uri 对象：

<!-- library-tour/parse-uri/bin/main.dart -->
{% prettify dart %}
var uri = Uri.parse('http://example.org:8080/foo/bar#frag');

assert(uri.scheme   == 'http');
assert(uri.host     == 'example.org');
assert(uri.path     == '/foo/bar');
assert(uri.fragment == 'frag');
assert(uri.origin   == 'http://example.org:8080');
{% endprettify %}

#### Building URIs

使用 `Uri()` 构造函数可以从 URI 的
各个部分来构造一个 Uri 对象：

<!-- library-tour/uri_from_components/bin/main.dart -->
{% prettify dart %}
var uri = new Uri(scheme: 'http', host: 'example.org',
                  path: '/foo/bar', fragment: 'frag');
assert(uri.toString() ==
    'http://example.org/foo/bar#frag');
{% endprettify %}

更多信息参考
 [Uri API 文档]({{site.dart_api}}/dart-core-Uri-class.html) 。

### Dates and times

DateTime 对象代表某个时刻。时区是 UTC 或者
本地时区。

一些构造函数可以创建 DateTime 对象：

<!-- library-tour/date/bin/main.dart -->
{% prettify dart %}
// Get the current date and time.
var now = new DateTime.now();

// Create a new DateTime with the local time zone.
var y2k = new DateTime(2000);   // January 1, 2000

// Specify the month and day.
y2k = new DateTime(2000, 1, 2); // January 2, 2000

// Specify the date as a UTC time.
y2k = new DateTime.utc(2000);   // 1/1/2000, UTC

// Specify a date and time in ms since the Unix epoch.
y2k = new DateTime.fromMillisecondsSinceEpoch(
    946684800000, isUtc: true);

// Parse an ISO 8601 date.
y2k = DateTime.parse('2000-01-01T00:00:00Z');
{% endprettify %}

`millisecondsSinceEpoch` 属性返回自从 “Unix epoch”—January 1, 1970, UTC
以来的毫秒数值：

<!-- library-tour/date/bin/main.dart -->
{% prettify dart %}
// 1/1/2000, UTC
y2k = new DateTime.utc(2000);
assert(y2k.millisecondsSinceEpoch == 946684800000);

// 1/1/1970, UTC
var unixEpoch = new DateTime.utc(1970);
assert(unixEpoch.millisecondsSinceEpoch == 0);
{% endprettify %}

使用 Duration 类可以计算两个日期之间的间隔，
还可以前后位移日期：

<!-- library-tour/date/bin/main.dart -->
{% prettify dart %}
var y2k = new DateTime.utc(2000);

// Add one year.
var y2001 = y2k.add(const Duration(days: 366));
assert(y2001.year == 2001);

// Subtract 30 days.
var december2000 = y2001.subtract(
    const Duration(days: 30));
assert(december2000.year == 2000);
assert(december2000.month == 12);

// Calculate the difference between two dates.
// Returns a Duration object.
var duration = y2001.difference(y2k);
assert(duration.inDays == 366); // y2k was a leap year.
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**警告：**
使用 Duration 来在 DateTime 对象上前后移动数天可能会有问题，
比如像夏令时等时间问题。如果要按照天数来位移时间，则
需要使用 UTC 日期。
</div>

更多详细信息参考
[DateTime]({{site.dart_api}}/dart-core/DateTime-class.html) 和
[Duration]({{site.dart_api}}/dart-core/Duration-class.html) 的 API
文档。


### Utility classes（工具类）

核心库还包含了一些常用的工具类，比如排序、
值映射以及遍历数据等。

#### Comparing objects（比较对象）

实现
[Comparable]({{site.dart_api}}/dart-core/Comparable-class.html)
接口表明该对象可以相互比较，通常用来
排序。`compareTo()` 函数对于 *小于的值*返回 \< 0 ；
*相同的值*返回 0 ； *大于的值*返回 \> 0、

<!-- library-tour/comparable/bin/main.dart -->
{% prettify dart %}
class Line implements Comparable {
  final length;
  const Line(this.length);
  int compareTo(Line other) => length - other.length;
}

main() {
  var short = const Line(1);
  var long = const Line(100);
  assert(short.compareTo(long) < 0);
}
{% endprettify %}

#### Implementing map keys

Dart 中的每个对象都有一个整数 hash 码，这样每个对象都
可以当做 map 的 key 来用。但是，你可以覆写
`hashCode` getter 来自定义 hash 码的实现，如果你这样做了，你也需要
同时覆写 `==` 操作符。相等的对象（使用 `==` 比较）的
 hash 码应该一样。Hasm 码并不要求是唯一的，
 但是应该具有良好的分布形态。

{% comment %}
Note: There’s disagreement over whether to include identical() in the ==
implementation. It might improve speed, at least when you need to
compare many fields. They don’t do identical() automatically because, by
convention, NaN != NaN.
{% endcomment %}

<!-- library-tour/map-keys/bin/main.dart -->
{% prettify dart %}
class Person {
  final String firstName, lastName;

  Person(this.firstName, this.lastName);

  // Override hashCode using strategy from Effective Java,
  // Chapter 11.
  int get hashCode {
    int result = 17;
    result = 37 * result + firstName.hashCode;
    result = 37 * result + lastName.hashCode;
    return result;
  }

  // You should generally implement operator == if you
  // override hashCode.
  bool operator ==(other) {
    if (other is! Person) return false;
    Person person = other;
    return (person.firstName == firstName &&
        person.lastName == lastName);
  }
}

main() {
  var p1 = new Person('bob', 'smith');
  var p2 = new Person('bob', 'smith');
  var p3 = 'not a person';
  assert(p1.hashCode == p2.hashCode);
  assert(p1 == p2);
  assert(p1 != p3);
}
{% endprettify %}

#### Iteration

[Iterable]({{site.dart_api}}/dart-core/Iterable-class.html) 和
[Iterator]({{site.dart_api}}/dart-core/Iterator-class.html) 类支持
for-in 循环。当你创建一个类的时候，继承或者实现 Iterable 可以
提供一个用于 for-in 循环的 Iterators。
实现 Iterator 来定义实际的遍历操作。

<!-- library-tour/iterator/bin/main.dart -->
{% prettify dart %}
class Process {
  // Represents a process...
}

class ProcessIterator implements Iterator<Process> {
  Process current;
  bool moveNext() {
    return false;
  }
}

// A mythical class that lets you iterate through all
// processes. Extends a subclass of Iterable.
class Processes extends IterableBase<Process> {
  final Iterator<Process> iterator =
      new ProcessIterator();
}

main() {
  // Iterable objects can be used with for-in.
  for (var process in new Processes()) {
    // Do something with the process.
  }
}
{% endprettify %}


### Exceptions

Dart 核心库定义了很多常见的异常和错误类。
异常通常是一些可以预知和预防的例外情况。
错误这是无法预料的并且不需要预防的情况。

下面是一些常见的错误：

[NoSuchMethodError]({{site.dart_api}}/dart-core/NoSuchMethodError-class.html)

:   当在一个对象上调用一个该对象没有
    实现的函数会抛出该错误。

[ArgumentError]({{site.dart_api}}/dart-core/ArgumentError-class.html)

:   调用函数的参数不合法会抛出这个错误。

抛出一个应用相关的异常是一种用来表明有错误发生的常见
方法。你还可以通过实现 Exception 接口来自定义
一些异常。

<!-- library-tour/reference/exceptions.dart -->
{% prettify dart %}
class FooException implements Exception {
  final String msg;
  const FooException([this.msg]);
  String toString() => msg ?? 'FooException';
}
{% endprettify %}

更多信息，请参考 [Exceptions](#exceptions) 和 [Exception API
文档]({{site.dart_api}}/dart-core/Exception-class.html)。


## dart:async - asynchronous programming

异步编程通常使用回调函数，但是 Dart 提供了另外的
选择：
[Future]({{site.dart_api}}/dart-async/Future-class.html) 和
[Stream]({{site.dart_api}}/dart-async/Stream-class.html) 对象。
Future 和 JavaScript 中的 Promise 类似，代表在将来某个时刻会返回一个
结果。Stream 是一种用来获取一些列数据的方式，例如 事件流。
Future, Stream, 以及其他异步操作的类在 
[dart:async]({{site.dart_api}}/dart-async/dart-async-library.html) 库中。

<div class="alert alert-info" markdown="1">
**注意：**
你并不是都需要直接使用 Future 和 Stream。
Dart 语言有一些异步功能的关键字，例如
`async` 和 `await`。
详细信息
 请参考 [异步支持](/guides/language/language-tour#asynchrony-support)。
</div>

dart:async 库在 web app 和命令行 app 都可以使用。
只需要导入 dart:async 即可使用：

<!-- library-tour/futures/bin/main.dart -->
{% prettify dart %}
import 'dart:async';
{% endprettify %}


### Future

在 Dart 库中随处可见 Future 对象，通常异步函数返回的对象就是一个 Future。
当一个 future *执行完后*，他里面的值
就可以使用了。

#### Using await

在直接使用 Future api 之前，你可以考虑先使用 `await`。
使用 `await` 的表达式比直接使用 Future api 的代码要更加
容易理解。

例如下面的方法。使用 Future 的 `then()` 函数来
执行三个异步方法，
每个方法执行完后才继续执行后一个方法。

<!-- library-tour/async-await/bin/main.dart -->
{% prettify dart %}
runUsingFuture() {
  //...
  findEntrypoint().then((entrypoint) {
    return runExecutable(entrypoint, args);
  }).then(flushThenExit);
}
{% endprettify %}

下面是使用 await 表达式实现的同样功能的代码，
看起来更像是同步代码，更加容易理解：

<!-- library-tour/async-await/bin/main.dart -->
{% prettify dart %}
runUsingAsyncAwait() async {
  //...
  var entrypoint = await findEntrypoint();
  var exitCode = await runExecutable(entrypoint, args);
  await flushThenExit(exitCode);
}
{% endprettify %}

异步方法可以把 Future 中的错误当做 异常来处理。
例如：

<!-- dart-tutorials-samples/count_down/tute_countdown.dart -->
<!-- This Polymer example has been removed, but I'll leave this code... -->
{% prettify dart %}
attached() async {
  super.attached();
  try {
    await appObject.start();
  } catch (e) {
    //...handle the error...
  }
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**重要：**
异步方法（带有关键字 async 的方法）会返回 Future。
如果你不想让你的方法返回 future，则
不要使用 async 关键字。
例如，你可以在你的方法里面调用一个 异步方法。
</div>

关于使用 `await` 和相关 Dart 语言的其他特性，请参考
[异步支持](/guides/language/language-tour#asynchrony-support)。


#### Basic usage

{% comment %}
[PENDING: Delete much of the following content in favor of the tutorial coverage?]
{% endcomment %}

可以使用 `then()` 来在 future 完成的时候执行其他代码。例如
`HttpRequest.getString()` 返回一个Future，由于 HTTP 请求是一个
耗时操作。使用 `then()` 可以在 Future 完成的时候执行其他代码
来解析返回的数据：

<!-- library-tour/async-await/async-await-web/web/main.dart -->
{% prettify dart %}
HttpRequest.getString(url).then((String result) {
  print(result);
});
// Should handle errors here.
{% endprettify %}

使用 `catchError()` 来处理 Future 对象可能抛出的
各种异常和错误：

<!-- library-tour/async-await/async-await/web/web/main.dart -->
{% prettify dart %}
HttpRequest.getString(url).then((String result) {
  print(result);
}).catchError((e) {
  // Handle or ignore the error.
});
{% endprettify %}

`then().catchError()`  模式就是异步版本的
`try`-`catch`。

<div class="alert alert-warning" markdown="1">
**重要：**
确保是在 `then()` 返回的 Future 上调用 `catchError()`，
而不是在 原来的 Future 对象上调用。否则的话，`catchError()`
就只能处理原来 Future 对象抛出的异常而无法处理  `then()` 代码
里面的异常。
</div>

#### Chaining multiple asynchronous methods

`then()` 函数返回值为 Future，可以把多个异步调用给串联起来。
如果 `then()` 函数注册的回调函数也返回一个 Future，而 `then()` 
返回一个同样的 Future。如果回调函数返回的是一个其他类型的值，
则 `then()` 会创建一个新的 Future 对象
并完成这个 future。

<!-- library-tour/futures/bin/main.dart -->
{% prettify dart %}
Future result = costlyQuery();

return result.then((value) => expensiveWork())
             .then((value) => lengthyComputation())
             .then((value) => print('done!'))
             .catchError((exception) => print('DOH!'));
{% endprettify %}

上面的示例中，代码是按照如下顺序执行的：

1.  `costlyQuery()`
2.  `expensiveWork()`
3.  `lengthyComputation()`


#### Waiting for multiple futures

有时候，你的算法要求调用很多异步方法，并且等待
所有方法完成后再继续执行。使用
[`Future.wait()`]({{site.dart_api}}/dart-async/Future/wait.html)
这个静态函数来管理多个 Future 并等待所有 Future 执行完成。

<!-- library-tour/futures/bin/main.dart -->
{% prettify dart %}
Future deleteDone = deleteLotsOfFiles();
Future copyDone = copyLotsOfFiles();
Future checksumDone = checksumLotsOfOtherFiles();

Future.wait([deleteDone, copyDone, checksumDone])
    .then((List values) {
      print('Done with all the long steps');
    });
{% endprettify %}


### Stream

Stream 在 Dart API 中也经常出现，代表一些列数据。
例如， HTML 按钮点击事件就可以使用 stream 来表示。
还可以把读取文件内容当做一个 Stream。


#### Using an asynchronous for loop

有时候可以使用异步 for 循环(`await for`)来
替代 Stream API。

例如下面的示例中，使用
Stream 的 `listen()` 函数来订阅
一些文件，然后使用一个方法参数来
搜索每个文件和目录。

<!-- OLD dart-tutorials-samples/cmdline/bin/dgrep.dart -->
{% prettify dart %}
void main(List<String> arguments) {
  ...
  FileSystemEntity.isDirectory(searchPath).then((isDir) {
    if (isDir) {
      final startingDir = new Directory(searchPath);
      startingDir
          .list(
              recursive: argResults[RECURSIVE],
              followLinks: argResults[FOLLOW_LINKS])
          .listen((entity) {
        if (entity is File) {
          searchFile(entity, searchTerms);
        }
      });
    } else {
      searchFile(new File(searchPath), searchTerms);
    }
  });
}
{% endprettify %}

下面是使用 await 表达式和异步 for 循环
实现的等价的代码，
看起来更像是同步代码：

<!-- dart-tutorials-samples/cmdline/bin/dgrep.dart -->
{% prettify dart %}
main(List<String> arguments) async {
  ...
  if (await FileSystemEntity.isDirectory(searchPath)) {
    final startingDir = new Directory(searchPath);
    await for (var entity in startingDir.list(
        recursive: argResults[RECURSIVE],
        followLinks: argResults[FOLLOW_LINKS])) {
      if (entity is File) {
        searchFile(entity, searchTerms);
      }
    }
  } else {
    searchFile(new File(searchPath), searchTerms);
  }
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**重要：**
在使用 `await for` 之前请确保使用之后的代码看起来确实是
更加清晰易懂了。例如，对于 DOM时间监听器通常
**不会**使用 `await for`，原因在于 DOM 发送
无尽的事件流。
如果使用 `await for` 来在同一行注册两个 DOM 事件监听器，
则第二个事件在不会被处理。
</div>

关于使用 `await` 和相关 Dart 语言的其他特性，
请参考
[异步支持](/guides/language/language-tour#asynchrony-support)。


#### Listening for stream data

要想在每个数据到达的时候就去处理，则可以选择使用 `await for` 或者
使用 `listen()` 函数来订阅事件：

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
// Find a button by ID and add an event handler.
querySelector('#submitInfo').onClick.listen((e) {
  // When the button is clicked, it runs this code.
  submitData();
});
{% endprettify %}

上面的示例中， `onClick` 属性是 "submitInfo" 按钮提供的一个
Stream 对象。

如果你只关心里面其中一个事件，则可以使用这些属性：
 `first`, `last`, 或者 `single`。要想在处理事件之前先测试是否满足条件，则
使用 `firstWhere()`, `lastWhere()`, 或者 `singleWhere()` 函数。
{% comment %}
{PENDING: example}
{% endcomment %}

如果你关心一部分事件，则可以使用
`skip()`, `skipWhile()`, `take()`, `takeWhile()`, 和 `where()` 这些函数。
{% comment %}
{PENDING: example}
{% endcomment %}


#### Transforming stream data

经常你需要先转换 stream 里面的数据才能使用。
使用 `transform()` 函数可以生产另外一个数据类型
的 Stream 对象：

<!-- library-tour/read-file/read_file.dart -->
{% prettify dart %}
var stream = inputStream
    .transform(UTF8.decoder)
    .transform(new LineSplitter());
{% endprettify %}

上面的代码使用两种转换器（transformer）。第一个使用 UTF8.decoder 
来把整数类型的数据流转换为字符串类型的数据流。然后使用
LineSplitter 把字符串类型数据流转换为按行分割的数据流。
这些转换器都来至于  dart:convert 库。
参考 [dart:convert ](#dartconvert---decoding-and-encoding-json-utf-8-and-more)) 了解详情。
{% comment %}
  PENDING: add onDone and onError. (See "Streaming file contents".)
{% endcomment %}


#### Handling errors and completion

使用异步 for 循环 (`await for`) 和使用 Stream API 的
异常处理情况是有
区别的。

如果是异步 for 循环，则可以
使用 try-catch 来处理异常。

在 stream 关闭后执行的代码位于异步 for 循环
之后。

<!-- library-tour/read-file/read_file.dart -->
{% prettify dart %}
readFileAwaitFor() async {
  var config = new File('config.txt');
  Stream<List<int>> inputStream = config.openRead();

  var lines = inputStream
      .transform(UTF8.decoder)
      .transform(new LineSplitter());
  [[highlight]]try {[[/highlight]]
    await for (var line in lines) {
      print('Got ${line.length} characters from stream');
    }
    print('file is now closed');
  [[highlight]]}[[/highlight]] [[highlight]]catch (e) {[[/highlight]]
    print(e);
  [[highlight]]}[[/highlight]]
}{% endprettify %}

If you use the Stream API,
then handle errors by registering an `onError` listener.
Run code after the stream is closed by registering
an `onDone` listener.

<!-- library-tour/read-file/read_file.dart -->
{% prettify dart %}
var config = new File('config.txt');
Stream<List<int>> inputStream = config.openRead();

inputStream
    .transform(UTF8.decoder)
    .transform(new LineSplitter())
    .listen((String line) {
      print('Got ${line.length} characters from stream');
    }, [[highlight]]onDone: () {[[/highlight]]
      print('file is now closed');
    [[highlight]]}[[/highlight]], [[highlight]]onError: (e) {[[/highlight]]
      print(e);
    [[highlight]]}[[/highlight]]);
{% endprettify %}


### More information

For some examples of using Future and Stream in command-line apps, see the
[dart:io section](#dartio---io-for-command-line-apps).
Also see these articles and tutorials:

-   [Asynchronous Programming: Futures](/tutorials/language/futures)

-   [Futures and Error Handling](/articles/libraries/futures-and-error-handling)

-   [The Event Loop and Dart]({{site.webdev}}/articles/performance/event-loop)

-   [Asynchronous Programming: Streams](/tutorials/language/streams)

-   [Creating Streams in Dart](/articles/libraries/creating-streams)


## dart:math - math and random

The Math library provides common functionality such as sine and cosine,
maximum and minimum, and constants such as *pi* and *e*. Most of the
functionality in the Math library is implemented as top-level functions.

To use the Math library in your app, import dart:math. The following
examples use the prefix `math` to make clear which top-level functions
and constants are from the Math library:

<!-- library-tour/math-tests/bin/main.dart -->
{% prettify dart %}
import 'dart:math' as math;
{% endprettify %}


### Trigonometry

The Math library provides basic trigonometric functions:

<!-- library-tour/math-tests/bin/main.dart -->
{% prettify dart %}
// Cosine
assert(math.cos(math.PI) == -1.0);

// Sine
var degrees = 30;
var radians = degrees * (math.PI / 180);
// radians is now 0.52359.
var sinOf30degrees = math.sin(radians);
// sin 30° = 0.5
assert((sinOf30degrees - 0.5).abs() < 0.01);
{% endprettify %}

<div class="alert alert-info" markdown="1">
**注意：**
These functions use radians, not degrees!
</div>


### Maximum and minimum

The Math library provides `max()` and `min()` methods:

<!-- library-tour/math-tests/bin/main.dart -->
{% prettify dart %}
assert(math.max(1, 1000) == 1000);
assert(math.min(1, -1000) == -1000);
{% endprettify %}


### Math constants

Find your favorite constants—*pi*, *e*, and more—in the Math library:

<!-- library-tour/math-tests/bin/main.dart -->
{% prettify dart %}
// See the Math library for additional constants.
print(math.E);     // 2.718281828459045
print(math.PI);    // 3.141592653589793
print(math.SQRT2); // 1.4142135623730951
{% endprettify %}


### Random numbers

Generate random numbers with the
[Random]({{site.dart_api}}/dart-math/Random-class.html) class. You can
optionally provide a seed to the Random constructor.

<!-- library-tour/math-tests/bin/main.dart -->
{% prettify dart %}
var random = new math.Random();
random.nextDouble(); // Between 0.0 and 1.0: [0, 1)
random.nextInt(10);  // Between 0 and 9.
{% endprettify %}

You can even generate random booleans:

<!-- library-tour/math-tests/bin/main.dart -->
{% prettify dart %}
var random = new math.Random();
random.nextBool();  // true or false
{% endprettify %}


### More information

Refer to the [Math API
docs]({{site.dart_api}}/dart-math/dart-math-library.html) for a full list of
methods. Also see the API docs for
[num,]({{site.dart_api}}/dart-core/num-class.html)
[int,]({{site.dart_api}}/dart-core/int-class.html) and
[double.]({{site.dart_api}}/dart-core/double-class.html)


## dart:html - browser-based apps

Use the [dart:html library]({{site.dart_api}}/dart-html/dart-html-library.html) to
program the browser, manipulate objects and elements in the DOM, and
access HTML5 APIs. DOM stands for *Document Object Model*, which
describes the hierarchy of an HTML page.

Other common uses of dart:html are manipulating styles (*CSS*), getting
data using HTTP requests, and exchanging data using
[WebSockets](#sending-and-receiving-real-time-data-with-websockets).
HTML5 (and dart:html) has many
additional APIs that this section doesn’t cover. Only web apps can use
dart:html, not command-line apps.

<div class="alert alert-info" markdown="1">
**注意：**
For higher level approaches to web app UIs, see
[Polymer Dart](https://github.com/dart-lang/polymer-dart/wiki) and
[Angular 2 for Dart](https://angular.io/dart).
</div>

To use the HTML library in your web app, import dart:html:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
import 'dart:html';
{% endprettify %}

### Manipulating the DOM

To use the DOM, you need to know about *windows*, *documents*,
*elements*, and *nodes*.

A [Window]({{site.dart_api}}/dart-html/Window-class.html) object represents
the actual window of the web browser. Each Window has a Document object,
which points to the document that's currently loaded. The Window object
also has accessors to various APIs such as IndexedDB (for storing data),
requestAnimationFrame (for animations), and more. In tabbed browsers,
each tab has its own Window object.

With the [Document]({{site.dart_api}}/dart-html/Document-class.html) object,
you can create and manipulate
[Elements]({{site.dart_api}}/dart-html/Element-class.html) within the
document. Note that the document itself is an element and can be
manipulated.

The DOM models a tree of
[Nodes.]({{site.dart_api}}/dart-html/Node-class.html) These nodes are often
elements, but they can also be attributes, text, comments, and other DOM
types. Except for the root node, which has no parent, each node in the
DOM has one parent and might have many children.

#### Finding elements

To manipulate an element, you first need an object that represents it.
You can get this object using a query.

Find one or more elements using the top-level functions
`querySelector()` and`
        querySelectorAll()`. You can query by ID, class, tag, name, or
any combination of these. The [CSS Selector Specification
guide](http://www.w3.org/TR/css3-selectors/) defines the formats of the
selectors such as using a \# prefix to specify IDs and a period (.) for
classes.

The `querySelector()` function returns the first element that matches
the selector, while `querySelectorAll()`returns a collection of elements
that match the selector.

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
// Find an element by id (an-id).
Element elem1 = querySelector('#an-id');

// Find an element by class (a-class).
Element elem2 = querySelector('.a-class');

// Find all elements by tag (<div>).
List<Element> elems1 = querySelectorAll('div');

// Find all text inputs.
  List<Element> elems2 =
      querySelectorAll('input[type="text"]');

// Find all elements with the CSS class 'class'
// inside of a <p> that is inside an element with
// the ID 'id'.
List<Element> elems3 = querySelectorAll('#id p.class');
{% endprettify %}

#### Manipulating elements

You can use properties to change the state of an element. Node and its
subtype Element define the properties that all elements have. For
example, all elements have `classes`, `hidden`, `id`, `style`, and
`title` properties that you can use to set state. Subclasses of Element
define additional properties, such as the `href` property of
[AnchorElement.]({{site.dart_api}}/dart-html/AnchorElement-class.html)

Consider this example of specifying an anchor element in HTML:

<!-- library-tour/future-then/web/main.dart -->
{% prettify html %}
<a id="example" href="http://example.com">link text</a>
{% endprettify %}

This \<a\> tag specifies an element with an `href` attribute and a text
node (accessible via a `text` property) that contains the string
“linktext”. To change the URL that the link goes to, you can use
AnchorElement’s `href` property:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
querySelector('#example').href = 'http://dartlang.org';
{% endprettify %}

Often you need to set properties on multiple elements. For example, the
following code sets the `hidden` property of all elements that have a
class of “mac”, “win”, or “linux”. Setting the `hidden` property to true
has the same effect as adding `display:none` to the CSS.

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
<!-- In HTML: -->
<p>
  <span class="linux">Words for Linux</span>
  <span class="macos">Words for Mac</span>
  <span class="windows">Words for Windows</span>
</p>

// In Dart:
final osList = ['macos', 'windows', 'linux'];

// In real code you'd programmatically determine userOs.
var userOs = 'linux';

for (var os in osList) { // For each possible OS...
  bool shouldShow = (os == userOs); // Matches user OS?

  // Find all elements with class=os. For example, if
  // os == 'windows', call querySelectorAll('.windows')
  // to find all elements with the class "windows".
  // Note that '.$os' uses string interpolation.
  for (var elem in querySelectorAll('.$os')) {
    elem.hidden = !shouldShow; // Show or hide.
  }
}
{% endprettify %}

When the right property isn’t available or convenient, you can use
Element’s `attributes` property. This property is a
`Map<String, String>`, where the keys are attribute names. For a list of
attribute names and their meanings, see the [MDN Attributes
page.](https://developer.mozilla.org/en/HTML/Attributes) Here’s an
example of setting an attribute’s value:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
elem.attributes['someAttribute'] = 'someValue';
{% endprettify %}

#### Creating elements

You can add to existing HTML pages by creating new elements and
attaching them to the DOM. Here’s an example of creating a paragraph
(\<p\>) element:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
var elem = new ParagraphElement();
elem.text = 'Creating is easy!';
{% endprettify %}

You can also create an element by parsing HTML text. Any child elements
are also parsed and created.

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
var elem2 =
    new Element.html('<p>Creating <em>is</em> easy!</p>');
{% endprettify %}

Note that elem2 is a ParagraphElement in the preceding example.

Attach the newly created element to the document by assigning a parent
to the element. You can add an element to any existing element’s
children. In the following example, `body` is an element, and its child
elements are accessible (as a List\<Element\>) from the `children`
property.

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
document.body.children.add(elem2);
{% endprettify %}

#### Adding, replacing, and removing nodes

Recall that elements are just a kind of node. You can find all the
children of a node using the `nodes` property of Node, which returns a
List\<Node\> (as opposed to `children`, which omits non-Element nodes).
Once you have this list, you can use the usual List methods and
operators to manipulate the children of the node.

To add a node as the last child of its parent, use the List `add()`
method:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
// Find the parent by ID, and add elem as its last child.
querySelector('#inputs').nodes.add(elem);
{% endprettify %}

To replace a node, use the Node `replaceWith()` method:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
// Find a node by ID, and replace it in the DOM.
querySelector('#status').replaceWith(elem);
{% endprettify %}

To remove a node, use the Node `remove()` method:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
// Find a node by ID, and remove it from the DOM.
querySelector('#expendable').remove();
{% endprettify %}

#### Manipulating CSS styles

CSS, or *cascading style sheets*, defines the presentation styles of DOM
elements. You can change the appearance of an element by attaching ID
and class attributes to it.

Each element has a `classes` field, which is a list. Add and remove CSS
classes simply by adding and removing strings from this collection. For
example, the following sample adds the `warning` class to an element:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
var element = querySelector('#message');
element.classes.add('warning');
{% endprettify %}

It’s often very efficient to find an element by ID. You can dynamically
set an element ID with the `id` property:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
var message = new DivElement();
message.id = 'message2';
message.text = 'Please subscribe to the Dart mailing list.';
{% endprettify %}

You can reduce the redundant text in this example by using method
cascades:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
var message = new DivElement()
    ..id = 'message2'
    ..text = 'Please subscribe to the Dart mailing list.';
{% endprettify %}

While using IDs and classes to associate an element with a set of styles
is best practice, sometimes you want to attach a specific style directly
to the element:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
message.style
    ..fontWeight = 'bold'
    ..fontSize = '3em';
{% endprettify %}

#### Handling events

To respond to external events such as clicks, changes of focus, and
selections, add an event listener. You can add an event listener to any
element on the page. Event dispatch and propagation is a complicated
subject; [research the
details](http://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)
if you’re new to web programming.

Add an event handler using
<code><em>element</em>.on<em>Event</em>.listen(<em>function</em>)</code>,
where <code><em>Event</em></code> is the event
name and <code><em>function</em></code> is the event handler.

For example, here’s how you can handle clicks on a button:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
// Find a button by ID and add an event handler.
querySelector('#submitInfo').onClick.listen((e) {
  // When the button is clicked, it runs this code.
  submitData();
});
{% endprettify %}

Events can propagate up and down through the DOM tree. To discover which
element originally fired the event, use `e.target`:

<!-- library-tour/future-then/web/main.dart -->
{% prettify dart %}
document.body.onClick.listen((e) {
  var clickedElem = e.target;
  print('You clicked the ${clickedElem.id} element.');
});
{% endprettify %}

To see all the events for which you can register an event listener, look
for "onEventType" properties in the API docs for
[Element]({{site.dart_api}}/dart-html/Element-class.html) and its
subclasses. Some common events include:

-   change

-   blur

-   keyDown

-   keyUp

-   mouseDown

-   mouseUp


### Using HTTP resources with HttpRequest

Formerly known as XMLHttpRequest, the
[HttpRequest]({{site.dart_api}}/HttpRequest-class.html) class
gives you access to HTTP resources from within your browser-based app.
Traditionally, AJAX-style apps make heavy use of HttpRequest. Use
HttpRequest to dynamically load JSON data or any other resource from a
web server. You can also dynamically send data to a web server.

The following examples assume all resources are served from the same web
server that hosts the script itself. Due to security restrictions in the
browser, the HttpRequest class can’t easily use resources that are
hosted on an origin that is different from the origin of the app. If you
need to access resources that live on a different web server, you need
to either use a technique called JSONP or enable CORS headers on the
remote resources.

#### Getting data from the server

The HttpRequest static method `getString()` is an easy way to get data
from a web server. Use `await` with the `getString()` call
to ensure that you have the data before continuing execution.

<!-- library-tour/get-data-from-server/web/main.dart -->
{% prettify dart %}
import 'dart:html';
import 'dart:async';

// A JSON-formatted file next to this page.
var uri = 'data.json';

main() async {
  // Read a JSON file.
  var data = await HttpRequest.getString(uri);
  processString(data);
}

processString(String jsonText) {
  parseText(jsonText);
}
{% endprettify %}

Information about the JSON API is in the
[dart:convert section](#dartconvert---decoding-and-encoding-json-utf-8-and-more).

{% comment %}
{PENDING: convert to async exception catcher?}
{% endcomment %}

Use try-catch to specify an error handler:

<!-- library-tour/get-data-from-server/web/main.dart -->
{% prettify dart %}
try {
  data = await HttpRequest.getString(jsonUri);
  processString(data);
} catch (e) {
  handleError(e);
}
// ...
handleError(error) {
  print('Uh oh, there was an error.');
  print(error.toString());
}
{% endprettify %}

If you need access to the HttpRequest, not just the text data it
retrieves, you can use the `request()` static method instead of
`getString()`. Here’s an example of reading XML data:

<!-- library-tour/get-data-from-server/web/main.dart -->
{% prettify dart %}
import 'dart:html';
import 'dart:async';

// An XML-formatted file next to this page.
var xmlUri = 'data.xml';

main() async {
  // Read an XML file.
  try {
    var data = await HttpRequest.request(xmlUri);
    processRequest(data);
  } catch (e) {
    handleError(e);
  }
}

processRequest(HttpRequest request) {
  var xmlDoc = request.responseXml;
  try {
    var license = xmlDoc.querySelector('license').text;
    print('License: $license');
  } catch (e) {
    print("$xmlUri doesn't have correct XML formatting.");
  }
}
{% endprettify %}

You can also use the full API to handle more interesting cases. For
example, you can set arbitrary headers.

The general flow for using the full API of HttpRequest is as follows:

1.  Create the HttpRequest object.
2.  Open the URL with either `GET` or `POST`.
3.  Attach event handlers.
4.  Send the request.

For example:

<!-- dart-tutorials-samples/web/portmanteaux/portmanteaux.dart -->
{% prettify dart %}
import 'dart:html';
// ...
var request = new HttpRequest()
    ..open('POST', dataUrl)
    ..onLoadEnd.listen((_) => requestComplete(request))
    ..send(encodedData);
{% endprettify %}

#### Sending data to the server

HttpRequest can send data to the server using the HTTP method POST. For
example, you might want to dynamically submit data to a form handler.
Sending JSON data to a RESTful web service is another common example.

Submitting data to a form handler requires you to provide name-value
pairs as URI-encoded strings. (Information about the URI class is in
the [URIs section](#uris).)
You must also set the `Content-type` header to
`application/x-www-form-urlencode` if you wish to send data to a form
handler.

<!-- library-tour/post-data-to-server/web/main.dart -->
{% prettify dart %}
import 'dart:html';

String encodeMap(Map data) {
  return data.keys.map((k) {
    return '${Uri.encodeComponent(k)}=' +
           '${Uri.encodeComponent(data[k])}';
  }).join('&');
}

loadEnd(HttpRequest request) {
  if (request.status != 200) {
    print('Uh oh, error: ${request.status}');
  } else {
    print('Data has been posted');
  }
}

main() async {
  var dataUrl = '/registrations/create';
  var data = {'dart': 'fun', 'editor': 'productive'};
  var encodedData = encodeMap(data);

  var httpRequest = new HttpRequest();
  httpRequest.open('POST', dataUrl);
  httpRequest.setRequestHeader(
      'Content-type',
      'application/x-www-form-urlencoded');
  httpRequest.send(encodedData);
  await httpRequest.onLoadEnd.first;
  loadEnd(httpRequest);
}
{% endprettify %}

{% comment %}
[PENDING: Test the above code!!! ]
{% endcomment %}


### Sending and receiving real-time data with WebSockets

A WebSocket allows your web app to exchange data with a server
interactively—no polling necessary. A server creates the WebSocket and
listens for requests on a URL that starts with **ws://**—for example,
ws://127.0.0.1:1337/ws. The data transmitted over a WebSocket can be a
string or a blob.  Often, the data is a JSON-formatted string.

To use a WebSocket in your web app, first create a
[WebSocket]({{site.dart_api}}/dart-html/WebSocket-class.html) object, passing
the WebSocket URL as an argument:

<!-- github.com/dart-lang/dart-samples/html5/web/websockets/basics/websocket_sample.dart -->
{% prettify dart %}
var ws = new WebSocket('ws://echo.websocket.org');
{% endprettify %}

#### Sending data

To send string data on the WebSocket, use the `send()` method:

<!-- github.com/dart-lang/dart-samples/html5/web/websockets/basics/websocket_sample.dart -->
{% prettify dart %}
ws.send('Hello from Dart!');
{% endprettify %}

#### Receiving data

To receive data on the WebSocket, register a listener for message
events:

<!-- github.com/dart-lang/dart-samples/html5/web/websockets/basics/websocket_sample.dart -->
{% prettify dart %}
ws.onMessage.listen((MessageEvent e) {
  print('Received message: ${e.data}');
});
{% endprettify %}

The message event handler receives a
[MessageEvent]({{site.dart_api}}/dart-html/MessageEvent-class.html) object.
This object’s `data` field has the data from the server.

#### Handling WebSocket events

Your app can handle the following WebSocket events: open, close, error,
and (as shown earlier) message. Here’s an example of a method that
creates a WebSocket object and registers handlers for open, close,
error, and message events:

<!-- https://github.com/dart-lang/dart-samples/blob/master/html5/web/websockets/basics/websocket_sample.dart -->
{% prettify dart %}
void initWebSocket([int retrySeconds = 2]) {
  var reconnectScheduled = false;

  print("Connecting to websocket");
  ws = new WebSocket('ws://echo.websocket.org');

  void scheduleReconnect() {
    if (!reconnectScheduled) {
      new Timer(
          new Duration(milliseconds: 1000 * retrySeconds),
          () => initWebSocket(retrySeconds * 2));
    }
    reconnectScheduled = true;
  }

  ws.onOpen.listen((e) {
    print('Connected');
    ws.send('Hello from Dart!');
  });

  ws.onClose.listen((e) {
    print('Websocket closed, retrying in ' +
          '$retrySeconds seconds');
    scheduleReconnect();
  });

  ws.onError.listen((e) {
    print("Error connecting to ws");
    scheduleReconnect();
  });

  ws.onMessage.listen((MessageEvent e) {
    print('Received message: ${e.data}');
  });
}
{% endprettify %}


### More information

This section barely scratched the surface of using the dart:html
library. For more information, see the documentation for
[dart:html]({{site.dart_api}}/dart-html/dart-html-library.html).
Dart has additional libraries for more specialized web APIs, such as [web
audio,]({{site.dart_api}}/dart-web_audio/dart-web_audio-library.html)
[IndexedDB]({{site.dart_api}}/dart-indexed_db/dart-indexed_db-library.html), and
[WebGL]({{site.dart_api}}/dart-web_gl/dart-web_gl-library.html).


## dart:io - I/O for command-line apps

The [dart:io library]({{site.dart_api}}/dart-io/dart-io-library.html) provides APIs to
deal with files, directories, processes, sockets, WebSockets, and HTTP
clients and servers. Only command-line apps can use dart:io—not web
apps.

In general, the dart:io library implements and promotes an asynchronous
API. Synchronous methods can easily block an application, making it
difficult to scale. Therefore, most operations return results via Future
or Stream objects, a pattern common with modern server platforms such as
Node.js.

The few synchronous methods in the dart:io library are clearly marked
with a Sync suffix on the method name. We don’t cover them here.

<div class="alert alert-info" markdown="1">
**注意：**
Only command-line apps can import and use `dart:io`.
</div>


### Files and directories

The I/O library enables command-line apps to read and write files and
browse directories. You have two choices for reading the contents of a
file: all at once, or streaming. Reading a file all at once requires
enough memory to store all the contents of the file. If the file is very
large or you want to process it while reading it, you should use a
Stream, as described in
[Streaming file contents](#streaming-file-contents).

#### Reading a file as text

When reading a text file encoded using UTF-8, you can read the entire
file contents with `readAsString()`. When the individual lines are
important, you can use `readAsLines()`. In both cases, a Future object
is returned that provides the contents of the file as one or more
strings.

<!-- library-tour/read-file/bin/text_read.dart -->
{% prettify dart %}
import 'dart:io';

main() async {
  var config = new File('config.txt');
  var contents;

  // Put the whole file in a single string.
  contents = await config.readAsString();
  print('The entire file is ${contents.length} characters long.');

  // Put each line of the file into its own string.
  contents = await config.readAsLines();
  print('The entire file is ${contents.length} lines long.');
}
{% endprettify %}


#### Reading a file as binary

The following code reads an entire file as bytes into a list of ints.
The call to `readAsBytes()` returns a Future, which provides the result
when it’s available.

<!-- library-tour/read-file/bin/binary_read.dart -->
{% prettify dart %}
import 'dart:io';

main() async {
  var config = new File('config.txt');

  var contents = await config.readAsBytes();
  print('The entire file is ${contents.length} bytes long');
}
{% endprettify %}

#### Handling errors

To capture errors so they don't result in uncaught exceptions, you can
register a `catchError` handler on the Future,
or (in an async function) use try-catch:

<!-- library-tour/read-file/bin/file_errors.dart -->
{% prettify dart %}
import 'dart:io';

main() async {
  var config = new File('config.txt');
  try {
    var contents = await config.readAsString();
    print(contents);
  } catch (e) {
    print(e);
  }
}
{% endprettify %}

#### Streaming file contents

Use a Stream to read a file, a little at a time.
You can use either the [Stream API](#stream) or `await for`,
part of Dart's [asynchrony support](/guides/language/language-tour#asynchrony-support).

<!-- library-tour/read-file/bin/read_file.dart -->
{% prettify dart %}
import 'dart:io';
import 'dart:convert';
import 'dart:async';

main() async {
  var config = new File('config.txt');
  Stream<List<int>> inputStream = config.openRead();

  var lines = inputStream
      .transform(UTF8.decoder)
      .transform(new LineSplitter());
  try {
    await for (var line in lines) {
      print('Got ${line.length} characters from stream');
    }
    print('file is now closed');
  } catch (e) {
    print(e);
  }
}
{% endprettify %}

#### Writing file contents

You can use an [IOSink]({{site.dart_api}}/dart-io/IOSink-class.html) to
write data to a file. Use the File `openWrite()` method to get an IOSink
that you can write to. The default mode, `FileMode.WRITE`, completely
overwrites existing data in the file.

<!-- library-tour/write-file/bin/main.dart -->
{% prettify dart %}
var logFile = new File('log.txt');
var sink = logFile.openWrite();
sink.write('FILE ACCESSED ${new DateTime.now()}\n');
sink.close();
{% endprettify %}

To add to the end of the file, use the optional `mode` parameter to
specify `FileMode.APPEND`:

<!-- library-tour/write_file/bin/main.dart -->
{% prettify dart %}
var sink = logFile.openWrite(mode: FileMode.APPEND);
{% endprettify %}

To write binary data, use `add(List<int> data)`.


#### Listing files in a directory

Finding all files and subdirectories for a directory is an asynchronous
operation. The `list()` method returns a Stream that emits an object
when a file or directory is encountered.

<!-- library-tour/list-files/bin/main.dart -->
{% prettify dart %}
import 'dart:io';

main() async {
  var dir = new Directory('/tmp');

  try {
    var dirList = dir.list();
    await for (FileSystemEntity f in dirList) {
      if (f is File) {
        print('Found file ${f.path}');
      } else if (f is Directory) {
        print('Found dir ${f.path}');
      }
    }
  } catch (e) {
    print(e.toString());
  }
}
{% endprettify %}


#### Other common functionality

The File and Directory classes contain other functionality, including
but not limited to:

-   Creating a file or directory: `create()` in File and Directory

-   Deleting a file or directory: `delete()` in File and Directory

-   Getting the length of a file: `length()` in File

-   Getting random access to a file: `open()` in File

Refer to the API docs for [File]({{site.dart_api}}/dart-io/File-class.html)
and [Directory]({{site.dart_api}}/dart-io/Directory-class.html) for a full
list of methods.


### HTTP clients and servers

The dart:io library provides classes that command-line apps can use for
accessing HTTP resources, as well as running HTTP servers.

#### HTTP server

The [HttpServer]({{site.dart_api}}/dart-io/HttpServer-class.html) class
provides the low-level functionality for building web servers. You can
match request handlers, set headers, stream data, and more.

The following sample web server can return only simple text information.
This server listens on port 8888 and address 127.0.0.1 (localhost),
responding to requests for the path `/languages/dart`. All other
requests are handled by the default request handler, which returns a
response code of 404 (not found).

<!-- library-tour/client-server/bin/http_server.dart -->
{% prettify dart %}
import 'dart:io';

main() async {
  dartHandler(HttpRequest request) {
    request.response.headers.contentType =
        new ContentType('text', 'plain');
    request.response.write('Dart is optionally typed');
    request.response.close();
  }

  var requests = await HttpServer.bind('127.0.0.1', 8888);
  await for (var request in requests) {
    print('Got request for ${request.uri.path}');
    if (request.uri.path == '/languages/dart') {
      dartHandler(request);
    } else {
      request.response.write('Not found');
      request.response.close();
    }
  }
}
{% endprettify %}

#### HTTP client

The [HttpClient]({{site.dart_api}}/dart-io/HttpClient-class.html) class
helps you connect to HTTP resources from your Dart command-line or
server-side application. You can set headers, use HTTP methods, and read
and write data. The HttpClient class does not work in browser-based
apps. When programming in the browser, use the [HttpRequest
class](#using-http-resources-with-httprequest).
Here’s an example of using HttpClient:

<!-- library-tour/client-server/bin/main.dart -->
{% prettify dart %}
import 'dart:io';
import 'dart:convert';

main() async {
  var url = Uri.parse(
      'http://127.0.0.1:8888/languages/dart');
  var httpClient = new HttpClient();
  var request = await httpClient.getUrl(url);
  print('have request');
  var response = await request.close();
  print('have response');
  var data = await response.transform(UTF8.decoder).toList();
  var body = data.join('');
  print(body);
  httpClient.close();
}
{% endprettify %}


### More information

Besides the APIs discussed in this section, the dart:io library also
provides APIs for [processes,]({{site.dart_api}}/dart-io/Process-class.html)
[sockets,]({{site.dart_api}}/dart-io/Socket-class.html) and [web
sockets.]({{site.dart_api}}/dart-io/Socket-class.html)

## dart:convert - decoding and encoding JSON, UTF-8, and more

The [dart:convert library]({{site.dart_api}}/dart-convert/dart-convert-library.html)
has converters for JSON and UTF-8, as well as support for creating
additional converters. JSON is a simple text format for representing
structured objects and collections. UTF-8 is a common variable-width
encoding that can represent every character in the Unicode character
set.

The dart:convert library works in both web apps and command-line apps.
To use it, import dart:convert.


### Decoding and encoding JSON

Decode a JSON-encoded string into a Dart object with `JSON.decode()`:

<!-- library-tour/json-parse/bin/main.dart -->
{% prettify dart %}
import 'dart:convert' show JSON;

main() {
  // NOTE: Be sure to use double quotes ("),
  // not single quotes ('), inside the JSON string.
  // This string is JSON, not Dart.
  var jsonString = '''
  [
    {"score": 40},
    {"score": 80}
  ]
  ''';

  var scores = JSON.decode(jsonString);
  assert(scores is List);

  var firstScore = scores[0];
  assert(firstScore is Map);
  assert(firstScore['score'] == 40);
}
{% endprettify %}

Encode a supported Dart object into a JSON-formatted string with
`JSON.encode()`:

<!-- library-tour/json_stringify/bin/main.dart -->
{% prettify dart %}
import 'dart:convert' show JSON;

main() {
  var scores = [
    {'score': 40},
    {'score': 80},
    {'score': 100, 'overtime': true, 'special_guest': null}
  ];

  var jsonText = JSON.encode(scores);
  assert(jsonText == '[{"score":40},{"score":80},'
                     '{"score":100,"overtime":true,'
                     '"special_guest":null}]');
}
{% endprettify %}

Only objects of type int, double, String, bool, null, List, or Map (with
string keys) are directly encodable into JSON. List and Map objects are
encoded recursively.

You have two options for encoding objects that aren't directly
encodable. The first is to invoke `encode()` with a second argument: a
function that returns an object that is directly encodable. Your second
option is to omit the second argument, in which case the encoder calls
the object's `toJson()` method.


### Decoding and encoding UTF-8 characters

Use `UTF8.decode()` to decode UTF8-encoded bytes to a Dart string:

<!-- library-tour/decode_utf-8/bin/main.dart -->
{% prettify dart %}
import 'dart:convert' show UTF8;

main() {
  var string = UTF8.decode([
    0xc3, 0x8e, 0xc3, 0xb1, 0xc5, 0xa3, 0xc3, 0xa9,
    0x72, 0xc3, 0xb1, 0xc3, 0xa5, 0xc5, 0xa3, 0xc3,
    0xae, 0xc3, 0xb6, 0xc3, 0xb1, 0xc3, 0xa5, 0xc4,
    0xbc, 0xc3, 0xae, 0xc5, 0xbe, 0xc3, 0xa5, 0xc5,
    0xa3, 0xc3, 0xae, 0xe1, 0xbb, 0x9d, 0xc3, 0xb1
  ]);
  print(string); // 'Îñţérñåţîöñåļîžåţîờñ'
}
{% endprettify %}

To convert a stream of UTF-8 characters into a Dart string, specify
`UTF8.decoder` to the Stream `transform()` method:

<!-- library-tour/read-file/read_file.dart -->
{% prettify dart %}
var lines = inputStream
    .transform(UTF8.decoder)
    .transform(new LineSplitter());
try {
  await for (var line in lines) {
    print('Got ${line.length} characters from stream');
  }
{% endprettify %}

Use `UTF8.encode()` to encode a Dart string as a list of UTF8-encoded
bytes:

<!-- library-tour/encode_utf-8/bin/main.dart -->
{% prettify dart %}
import 'dart:convert' show UTF8;

main() {
  List<int> expected = [
    0xc3, 0x8e, 0xc3, 0xb1, 0xc5, 0xa3, 0xc3, 0xa9,
    0x72, 0xc3, 0xb1, 0xc3, 0xa5, 0xc5, 0xa3, 0xc3,
    0xae, 0xc3, 0xb6, 0xc3, 0xb1, 0xc3, 0xa5, 0xc4,
    0xbc, 0xc3, 0xae, 0xc5, 0xbe, 0xc3, 0xa5, 0xc5,
    0xa3, 0xc3, 0xae, 0xe1, 0xbb, 0x9d, 0xc3, 0xb1
  ];

  List<int> encoded = UTF8.encode('Îñţérñåţîöñåļîžåţîờñ');

  assert(() {
    if (encoded.length != expected.length) return false;
    for (int i = 0; i < encoded.length; i++) {
      if (encoded[i] != expected[i]) return false;
    }
    return true;
  });
}
{% endprettify %}


### Other functionality

The dart:convert library also has converters for ASCII and ISO-8859-1
(Latin1). For details, see the [API docs for the dart:convert
library.]({{site.dart_api}}/dart-convert/dart-convert-library.html)


## dart:mirrors - reflection

The dart:mirrors library provides basic reflection abilities to Dart.
Use mirrors to query the structure of your program and to dynamically
invoke functions or methods at runtime.

The dart:mirrors library works in both web apps and command-line apps.
To use it, import dart:mirrors.

<div class="alert alert-warning" markdown="1">
**警告：**
Using dart:mirrors can cause dart2js to generate very large JavaScript
files.

The current workaround is to add a `@MirrorsUsed` annotation before
the import of dart:mirrors. For details, see the
[MirrorsUsed]({{site.dart_api}}/dart-mirrors/MirrorsUsed-class.html)
API documentation. This workaround is very likely to change, as the
dart:mirrors library is still under development.
</div>


### Symbols

The mirror system represents the names of Dart declarations (classes,
fields, and so on) by instances of the class
[Symbol]({{site.dart_api}}/dart-core/Symbol-class.html). Symbols work
even in code where names have changed due to minification.

When you know the name of the symbol ahead of time, use a symbol
literal. This way, repeated uses of the same symbol can use the same
canonicalized instance. If the name of the symbol is determined
dynamically at runtime, use the Symbol constructor.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
import 'dart:mirrors';

// If the symbol name is known at compile time.
const className = #MyClass;

// If the symbol name is dynamically determined.
var userInput = askUserForNameOfFunction();
var functionName = new Symbol(userInput);
{% endprettify %}

During minification, a compiler might replace a symbol name with a
different (often smaller) name. To convert from a symbol back to a
string, use `MirrorSystem.getName()`. This function returns the correct
name, even if the code was minified.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
import 'dart:mirrors';

const className = #MyClass;
assert('MyClass' == MirrorSystem.getName(className));
{% endprettify %}


### Introspection

Use mirrors to introspect the running program's structure. You can
inspect classes, libraries, instances, and more.

The examples in this section use the following Person class:

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
class Person {
  String firstName;
  String lastName;
  int age;

  Person(this.firstName, this.lastName, this.age);

  String get fullName => '$firstName $lastName';

  void greet(String other) {
    print('Hello there, $other!');
  }
}
{% endprettify %}

To begin, you need to *reflect* on a class or object to get its
*mirror*.

#### Class mirrors

Reflect on a Type to get its ClassMirror.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
ClassMirror mirror = reflectClass(Person);

assert('Person' ==
    MirrorSystem.getName(mirror.simpleName));
{% endprettify %}

You can also call `runtimeType` to get a Type from an instance.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
var person = new Person('Bob', 'Smith', 33);
ClassMirror mirror = reflectClass(person.runtimeType);
assert('Person' ==
    MirrorSystem.getName(mirror.simpleName));
{% endprettify %}

Once you have a ClassMirror, you can get a class's constructors, fields,
and more. Here is an example of listing the constructors of a class.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
showConstructors(ClassMirror mirror) {
  var constructors = mirror.declarations.values
      .where((m) => m is MethodMirror && m.isConstructor);

  constructors.forEach((m) {
    print('The constructor ${m.simpleName} has '
          '${m.parameters.length} parameters.');
  });
}
{% endprettify %}

Here is an example of listing all of the fields declared by a class.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
showFields(ClassMirror mirror) {
  var fields = mirror.declarations.values
      .where((m) => m is VariableMirror);

  fields.forEach((VariableMirror m) {
    var finalStatus = m.isFinal ? 'final' : 'not final';
    var privateStatus = m.isPrivate ?
        'private' : 'not private';
    var typeAnnotation = m.type.simpleName;

    print('The field ${m.simpleName} is $privateStatus ' +
          'and $finalStatus and is annotated as ' +
          '$typeAnnotation.');
  });
}{% endprettify %}

For a full list of methods, consult the [API docs for
ClassMirror]({{site.dart_api}}/dart-mirrors/ClassMirror-class.html).

#### Instance mirrors

Reflect on an object to get an InstanceMirror.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
var p = new Person('Bob', 'Smith', 42);
InstanceMirror mirror = reflect(p);
{% endprettify %}

If you have an InstanceMirror and you want to get the object that it
reflects, use `reflectee`.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
var person = mirror.reflectee;
assert(identical(p, person));
{% endprettify %}


### Invocation

Once you have an InstanceMirror, you can invoke methods and call getters
and setters. For a full list of methods, consult the [API docs for
InstanceMirror]({{site.dart_api}}/dart-mirrors/InstanceMirror-class.html).

#### Invoke methods

Use InstanceMirror's `invoke()` method to invoke a method on an object.
The first parameter specifies the method to be invoked, and the second
is a list of positional arguments to the method. An optional third
parameter lets you specify named arguments.

<!-- library-tour/mirrors/bin/main.dart -->
{% prettify dart %}
var p = new Person('Bob', 'Smith', 42);
InstanceMirror mirror = reflect(p);

mirror.invoke(#greet, ['Shailen']);
{% endprettify %}

#### Invoke getters and setters

Use InstanceMirror's `getField()` and `setField()` methods to get and
set properties of an object.

<!-- library-tours/mirrors/bin/main.dart -->
{% prettify dart %}
var p = new Person('Bob', 'Smith', 42);
InstanceMirror mirror = reflect(p);

// Get the value of a property.
var fullName = mirror.getField(#fullName).reflectee;
assert(fullName == 'Bob Smith');

// Set the value of a property.
mirror.setField(#firstName, 'Mary');
assert(p.firstName == 'Mary');
{% endprettify %}


### More information

The article [Reflection in Dart with
Mirrors](/articles/libraries/reflection-with-mirrors) has
more information and examples. Also see the API docs for
[dart:mirror,]({{site.dart_api}}/dart-mirrors/dart-mirrors-library.html) especially
[MirrorsUsed]({{site.dart_api}}/dart-mirrors/MirrorsUsed-class.html),
[ClassMirror,]({{site.dart_api}}/dart-mirrors/ClassMirror-class.html)
and
[InstanceMirror.]({{site.dart_api}}/dart-mirrors/InstanceMirror-class.html)


## Summary

This page introduced you to the most commonly used functionality in
many of Dart’s built-in libraries. It didn’t cover all the built-in
libraries, however. Others that you might want to look into include
[dart:collection,]({{site.dart_api}}/dart-collection/dart-collection-library.html)
[dart:isolate,]({{site.dart_api}}/dart-isolate/dart-isolate-library.html) and
[dart:typed\_data.]({{site.dart_api}}/dart-typed_data/dart-typed_data-library.html) You
can get yet more libraries by using the pub tool, discussed in the next
page. The [args,](https://pub.dartlang.org/packages/args)
[logging,](https://pub.dartlang.org/packages/logging)
[polymer,](https://pub.dartlang.org/packages/polymer) and
[test](https://pub.dartlang.org/packages/test) libraries are just a
sampling of what you can install using pub.

To learn more about the Dart language, see
[A Tour of the Dart Language](/guides/language/language-tour).
