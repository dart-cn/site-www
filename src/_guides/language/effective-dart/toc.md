    {% comment %}
    This file is generated from the other files in this directory.
    To re-generate it, please run the following command from root of
    the project:

      $ dart deploy/effective-dart-rules/bin/main.dart

    {% endcomment %}
    
<div class='effective_dart--summary_column' markdown='1'>

### Style


**标识符**

* <a href='/guides/language/effective-dart/style#uppercamelcase-'><strong>要</strong> 使用 <code>UpperCamelCase</code> 风格来命名类型名称</a>
* <a href='/guides/language/effective-dart/style#lowercase_with_underscores-'><strong>要</strong> 使用 <code>lowercase_with_underscores</code> 风格来命名库和文件名名字。</a>
* <a href='/guides/language/effective-dart/style#lowercase_with_underscores-'><strong>要</strong> 使用 <code>lowercase_with_underscores</code> 风格命名导入的前缀。</a>
* <a href='/guides/language/effective-dart/style#lowercamelcase-'><strong>要</strong> 使用 <code>lowerCamelCase</code> 风格来命名其他的标识符。</a>
* <a href='/guides/language/effective-dart/style#lowercamelcase-'><strong>推荐</strong> 使用 <code>lowerCamelCase</code> 来命名常量。</a>
* <a href='/guides/language/effective-dart/style#section-1'><strong>要</strong> 把超过两个字母的缩略词和首字母缩略词当做一般单词来对待。</a>

**顺序**

* <a href='/guides/language/effective-dart/style#dart-'><strong>要</strong> 把 "dart:" 导入语句放到其他导入语句之前。</a>
* <a href='/guides/language/effective-dart/style#package-'><strong>要</strong> 把 "package:" 导入语句放到相对导入语句之前。</a>
* <a href='/guides/language/effective-dart/style#package-'><strong>推荐</strong> 把"第三方" "package:" 导入语句放到其他语句之前。</a>
* <a href='/guides/language/effective-dart/style#export'><strong>要</strong> 把导出（export）语句放到所有导入语句之后的部分。</a>
* <a href='/guides/language/effective-dart/style#section-3'><strong>要</strong> 按照字母顺序来排序每个部分中的语句。</a>

**格式化**

* <a href='/guides/language/effective-dart/style#section-5'><strong>避免</strong> 每行长度超过 80 字符。</a>
* <a href='/guides/language/effective-dart/style#section-6'><strong>要</strong> 在所有的控制结构上使用大括号。</a>
* <a href='/guides/language/effective-dart/style#dartfmt-'><strong>要</strong> 使用 <code>dartfmt</code> 来格式化您的代码</a>
* <a href='/guides/language/effective-dart/style#tabs-'><strong>不要</strong> 使用 tabs 。</a>
* <a href='/guides/language/effective-dart/style#section-7'><strong>要</strong> 在每个语句或者声明后面添加一个空行。</a>
* <a href='/guides/language/effective-dart/style#operator-setter-'><strong>不要</strong> 在声明函数名字、操作符(operator)和 setter 以及参数名字之间添加空格。</a>
* <a href='/guides/language/effective-dart/style#operator-'><strong>要</strong> 在关键字 <code>operator</code> 后面添加一个空格。</a>
* <a href='/guides/language/effective-dart/style#section-8'><strong>要</strong> 在二元和三元操作符之间添加空格。</a>
* <a href='/guides/language/effective-dart/style#map-'><strong>要</strong> 在 <code>,</code> 和 <code>:</code> 后面添加空格，当用作 map 和命名参数的情况下。</a>
* <a href='/guides/language/effective-dart/style#section-9'><strong>不要</strong> 在一元操作符前后添加空格。</a>
* <a href='/guides/language/effective-dart/style#in---'><strong>要</strong>在 <code>in</code> 关键字前后添加空格，在循环中的每个 <code>;</code> 后面也要添加空格。</a>
* <a href='/guides/language/effective-dart/style#section-10'><strong>要</strong> 在流程控制关键字后面使用一个空格。</a>
* <a href='/guides/language/effective-dart/style#section-11'><strong>不要</strong>在 <code>(</code>, <code>[</code>, 和 <code>{</code> 之后使用空格，也不要在 <code>)</code>, <code>]</code>, 和 <code>}</code> 之前使用空格。</a>
* <a href='/guides/language/effective-dart/style#section-12'><strong>要</strong> 在函数和方法体的 <code>{</code> 之前添加一个空格。</a>
* <a href='/guides/language/effective-dart/style#section-13'><strong>要</strong> 把开始的大括号 (<code>{</code>) 放到同一行上。</a>
* <a href='/guides/language/effective-dart/style#section-14'><strong>要</strong> 把二元符合放到多行表达式的前面一行的结尾。</a>
* <a href='/guides/language/effective-dart/style#section-15'><strong>要</strong> 把三元操作符放到多个表达式的下一行开始位置。</a>
* <a href='/guides/language/effective-dart/style#section-16'><strong>要</strong> 把 <code>.</code> 放到下一行开头当表达式换行的时候。</a>
* <a href='/guides/language/effective-dart/style#section-17'><strong>要</strong> 把构造函数初始化列表中的每个参数和值都放到同一行。</a>
* <a href='/guides/language/effective-dart/style#prefer-splitting-every-element-in-a-collection-literal-if-it-does-not-fit-on-one-line'><strong>推荐</strong> 当无法在一行写完集合的时候，把每个元素都用集合定义的方式来表达。（PREFER splitting every element in a collection literal if it does not fit on one line.）</a>
* <a href='/guides/language/effective-dart/style#section-18'><strong>要</strong> 用两个空格来缩进代码块和集合体。</a>
* <a href='/guides/language/effective-dart/style#switch-case--case-'><strong>要</strong> 缩进 switch case 两个空格， case 体四个空格。</a>
* <a href='/guides/language/effective-dart/style#section-19'><strong>要</strong> 只少使用两个空格来缩进多行函数级联调用。</a>
* <a href='/guides/language/effective-dart/style#section-20'><strong>推荐</strong> 使用四个空格来缩进同一行的换行。</a>

</div>
<div class='effective_dart--summary_column' markdown='1'>


### Documentation


**注释**

* <a href='/guides/language/effective-dart/documentation#section-1'><strong>要</strong> 按照句子的格式来格式化评论。</a>
* <a href='/guides/language/effective-dart/documentation#section-2'><strong>不要</strong> 使用块注释作为解释说明。</a>

**文档注释**

* <a href='/guides/language/effective-dart/documentation#section-4'><strong>要</strong> 使用 <code>///</code> 文档注释来注释成员和类型。</a>
* <a href='/guides/language/effective-dart/documentation#api-'><strong>推荐</strong> 为公开发布的 API 编写注释文档。</a>
* <a href='/guides/language/effective-dart/documentation#api-'><strong>考虑</strong> 为私有 API 编写注释文档。</a>
* <a href='/guides/language/effective-dart/documentation#section-5'><strong>要</strong> 把第一个语句定义为一个段落。</a>
* <a href='/guides/language/effective-dart/documentation#section-6'><strong>推荐</strong> 用第三人称来开始函数或者方法的文档注释。</a>
* <a href='/guides/language/effective-dart/documentation#gettersetter-'><strong>推荐</strong> 使用名词短语来开始变量、getter、setter 的注释。</a>
* <a href='/guides/language/effective-dart/documentation#section-7'><strong>推荐</strong> 使用名词短语来开始库和类型注释。</a>
* <a href='/guides/language/effective-dart/documentation#section-8'><strong>考虑</strong> 在文档注释中添加示例代码。</a>
* <a href='/guides/language/effective-dart/documentation#section-9'><strong>要</strong> 使用方括号在文档注释中引用作用域内的标识符。</a>
* <a href='/guides/language/effective-dart/documentation#section-10'><strong>要</strong> 使用散文的方式来描述参数、返回值以及异常信息。</a>
* <a href='/guides/language/effective-dart/documentation#section-11'><strong>避免</strong> 在注释文档中提供冗余的类型信息。</a>
* <a href='/guides/language/effective-dart/documentation#section-12'><strong>要</strong> 把注释文档放到注解之前。</a>

**Markdown**

* <a href='/guides/language/effective-dart/documentation#markdown'><strong>避免</strong> 过度使用 markdown。</a>
* <a href='/guides/language/effective-dart/documentation#html-'><strong>避免</strong> 使用 HTML 来格式化文字。</a>

**如何写注释**

* <a href='/guides/language/effective-dart/documentation#section-14'><strong>推荐</strong> 简洁.</a>
* <a href='/guides/language/effective-dart/documentation#section-15'><strong>避免</strong> 缩写和首字母缩略词（非常流行的除外）。</a>
* <a href='/guides/language/effective-dart/documentation#this--the-'><strong>推荐</strong> 使用 "this" 而不是 "the" 来引用实例成员。</a>

</div>
<div style='clear:both'></div>
<div class='effective_dart--summary_column' markdown='1'>


### Usage


**Strings**

* <a href='/guides/language/effective-dart/usage#section'><strong>要</strong> 使用相邻的字符串字面量定义来链接字符串。</a>
* <a href='/guides/language/effective-dart/usage#section-1'><strong>推荐</strong> 使用插值的形式来组合字符串和值。</a>
* <a href='/guides/language/effective-dart/usage#section-2'><strong>避免</strong> 在字符串插值中使用多余的大括号。</a>

**集合**

* <a href='/guides/language/effective-dart/usage#section-4'><strong>要</strong> 尽可能的使用集合字面量来定义集合。</a>
* <a href='/guides/language/effective-dart/usage#length-'><strong>不要</strong> 使用 <code>.length</code> 来判断集合是否为空。</a>
* <a href='/guides/language/effective-dart/usage#higher-order'><strong>考虑</strong> 使用高阶（higher-order）函数来转换集合数据。</a>
* <a href='/guides/language/effective-dart/usage#iterableforeach-'><strong>避免</strong> 在 <code>Iterable.forEach()</code> 中使用函数声明形式。</a>

**方法(Functions)**

* <a href='/guides/language/effective-dart/usage#section-5'><strong>要</strong> 用方法声明的形式来给方法起个名字。</a>
* <a href='/guides/language/effective-dart/usage#lambda--tear-off'><strong>不要</strong> 使用 lambda 表达式来替代 tear-off。</a>

**变量**

* <a href='/guides/language/effective-dart/usage#null'><strong>不要</strong> 显式的把变量初始化为 <code>null</code>。</a>
* <a href='/guides/language/effective-dart/usage#section-7'><strong>避免</strong> 保存可以计算的结果。</a>
* <a href='/guides/language/effective-dart/usage#section-8'><strong>考虑</strong> 省略局部变量的类型。</a>

**成员**

* <a href='/guides/language/effective-dart/usage#getter--setter'><strong>不要</strong> 创建没必要的 getter 和 setter。</a>
* <a href='/guides/language/effective-dart/usage#final-'><strong>推荐</strong> 使用 <code>final</code> 关键字来限定只读属性。</a>
* <a href='/guides/language/effective-dart/usage#section-10'><strong>考虑</strong> 用 <code>=&gt;</code> 来实现只有一个单一返回语句的函数。</a>
* <a href='/guides/language/effective-dart/usage#this-'><strong>不要</strong> 使用 <code>this.</code> ，除非遇到了变量冲突的情况。</a>
* <a href='/guides/language/effective-dart/usage#section-11'><strong>要</strong> 尽可能的在定义变量的时候初始化其值。</a>

**构造函数**

* <a href='/guides/language/effective-dart/usage#section-13'><strong>要</strong> 尽可能的使用初始化形式。</a>
* <a href='/guides/language/effective-dart/usage#section-14'><strong>不要</strong> 在初始化形式上定义类型。</a>
* <a href='/guides/language/effective-dart/usage#section-15'><strong>要</strong> 用 <code>;</code> 来替代空函数体的构造函数 <code>{}</code>。</a>
* <a href='/guides/language/effective-dart/usage#super-'><strong>要</strong> 把 <code>super()</code> 调用放到构造函数初始化列表之后调用。</a>

**错误处理**

* <a href='/guides/language/effective-dart/usage#on--catch'><strong>避免</strong> 使用没有 <code>on</code> 语句的 catch。</a>
* <a href='/guides/language/effective-dart/usage#on-'><strong>不要</strong> 丢弃没有使用 <code>on</code> 语句捕获的异常。</a>
* <a href='/guides/language/effective-dart/usage#error-'><strong>要</strong> 只在代表编程错误的情况下才抛出实现了 <code>Error</code> 的异常。</a>
* <a href='/guides/language/effective-dart/usage#error-'><strong>不要</strong> 显示的捕获 <code>Error</code> 或者其子类。</a>
* <a href='/guides/language/effective-dart/usage#rethrow-'><strong>要</strong> 使用 <code>rethrow</code> 来重新抛出捕获的异常。</a>

**异步**

* <a href='/guides/language/effective-dart/usage#asyncawait-'><strong>推荐</strong> 使用 async/await 而不是直接使用底层的特性。</a>
* <a href='/guides/language/effective-dart/usage#async-'><strong>不要</strong> 在没有有用效果的情况下使用 <code>async</code> 。</a>
* <a href='/guides/language/effective-dart/usage#stream'><strong>考虑</strong> 使用高阶函数来转换事件流（stream）</a>
* <a href='/guides/language/effective-dart/usage#completer--'><strong>避免</strong> 直接使用 Completer  。</a>

</div>
<div class='effective_dart--summary_column' markdown='1'>


### Design


**命名**

* <a href='/guides/language/effective-dart/design#section-1'><strong>要</strong> 使用一致的术语。</a>
* <a href='/guides/language/effective-dart/design#section-2'><strong>避免</strong> 缩写。</a>
* <a href='/guides/language/effective-dart/design#section-3'><strong>推荐</strong> 把最具描述性的名词放到最后。</a>
* <a href='/guides/language/effective-dart/design#section-4'><strong>考虑</strong> 尽量让代码看起来像普通的句子。</a>
* <a href='/guides/language/effective-dart/design#section-5'><strong>推荐</strong> 使用名词短语来命名不是布尔类型的变量和属性。</a>
* <a href='/guides/language/effective-dart/design#section-6'><strong>推荐</strong> 使用非命令式动词短语命名布尔类型的变量和属性。</a>
* <a href='/guides/language/effective-dart/design#section-7'><strong>考虑</strong> 省略命名布尔<em>参数</em>的动词。</a>
* <a href='/guides/language/effective-dart/design#section-8'><strong>推荐</strong> 使用命令式动词短语来命名带有副作用的函数或者方法。</a>
* <a href='/guides/language/effective-dart/design#section-9'><strong>考虑</strong> 使用名词短语或者非命令式动词短语命名返回数据为主要功能的方法或者函数。</a>
* <a href='/guides/language/effective-dart/design#to___-'><strong>推荐</strong> 使用 <code>to___()</code> 来命名把对象的状态转换到一个新的对象的函数。</a>
* <a href='/guides/language/effective-dart/design#as___-'><strong>推荐</strong> 使用 <code>as___()</code> 来命名把原来对象转换为另外一种表现形式的函数。</a>
* <a href='/guides/language/effective-dart/design#section-10'><strong>避免</strong> 在方法或者函数名称中描述参数。</a>

**库**

* <a href='/guides/language/effective-dart/design#section-12'><strong>推荐</strong> 使用私有声明。</a>

**类型**

* <a href='/guides/language/effective-dart/design#section-14'><strong>避免</strong> 定义使用简单的方法可以替代的只有一个成员的抽象类。</a>
* <a href='/guides/language/effective-dart/design#section-15'><strong>避免</strong> 定义只包含静态成员的类。</a>
* <a href='/guides/language/effective-dart/design#section-16'><strong>避免</strong> 继承一个不打算被继承的类。</a>
* <a href='/guides/language/effective-dart/design#section-17'><strong>要</strong> 在文档中表明你的类是否打算被继承。</a>
* <a href='/guides/language/effective-dart/design#mixin'><strong>避免</strong> 混入（mixin）一个不打算被混入的类。</a>
* <a href='/guides/language/effective-dart/design#mixin-'><strong>要</strong> 在文档中注明你的类是否打算当做 mixin 使用。</a>

**构造函数**

* <a href='/guides/language/effective-dart/design#section-1'><strong>推荐</strong> 使用构造函数而不是静态函数来创建对象。</a>
* <a href='/guides/language/effective-dart/design#const-'><strong>考虑</strong> 让构造函数为 <code>const</code> 的。</a>

**成员**

* <a href='/guides/language/effective-dart/design#final-'><strong>推荐</strong> 把成员变量或者顶级变量定义为 <code>final</code> 类型。</a>
* <a href='/guides/language/effective-dart/design#getter-'><strong>要</strong> 使用 getter 来定义访问属性的操作。</a>
* <a href='/guides/language/effective-dart/design#setter'><strong>要</strong> 对于本质上为修改对象属性的函数要使用 setter。</a>
* <a href='/guides/language/effective-dart/design#getter--setter-'><strong>不要</strong> 定义没有对应 getter 的 setter 函数。</a>
* <a href='/guides/language/effective-dart/design#bool-double-int--num--null'><strong>避免</strong> 在返回值类型为 <code>bool</code>, <code>double</code>, <code>int</code>, 或者 <code>num</code> 的函数中返回 <code>null</code>。</a>
* <a href='/guides/language/effective-dart/design#this-'><strong>避免</strong> 在函数中返回 <code>this</code> 只是为了串联调用函数。</a>

**类型注解**

* <a href='/guides/language/effective-dart/design#api-'><strong>要</strong> 在公开的 API 上指定类型。</a>
* <a href='/guides/language/effective-dart/design#setter-'><strong>不要</strong> 为 setter 指定返回类型。</a>
* <a href='/guides/language/effective-dart/design#section-22'><strong>推荐</strong> 为私有成员提供类型。</a>
* <a href='/guides/language/effective-dart/design#section-23'><strong>避免</strong> 在方法表达式上使用类型。</a>
* <a href='/guides/language/effective-dart/design#dynamic-'><strong>避免</strong> 在没必要的地方使用 <code>dynamic</code> 类型。</a>
* <a href='/guides/language/effective-dart/design#function-'><strong>避免</strong> 使用 <code>Function</code> 类型。</a>
* <a href='/guides/language/effective-dart/design#object--dynamic-'><strong>要</strong> 使用 <code>Object</code> 来替代 <code>dynamic</code> 来表示可以接受任意对象。</a>

**参数**

* <a href='/guides/language/effective-dart/design#section-25'><strong>避免</strong> 位置参数作为可选布尔参数。</a>
* <a href='/guides/language/effective-dart/design#section-26'><strong>避免</strong> 把用户想要忽略的参数放到位置可选参数的前列。</a>
* <a href='/guides/language/effective-dart/design#section-27'><strong>避免</strong> 使用强制无意义的参数。</a>
* <a href='/guides/language/effective-dart/design#section-28'><strong>要</strong> 使用包含开始位置并且不包含结束位置的范围参数。</a>

**相等判断**

* <a href='/guides/language/effective-dart/design#hashcode-'><strong>要</strong> 在覆写 <code>==</code> 的同时覆写 <code>hashCode</code> 。</a>
* <a href='/guides/language/effective-dart/design#section-30'><strong>要</strong> 安装算术相等要规则来实现你的 <code>==</code> 操作符。</a>
* <a href='/guides/language/effective-dart/design#section-31'><strong>避免</strong> 为可变对象自定义相等函数。</a>
* <a href='/guides/language/effective-dart/design#null'><strong>不要</strong> 在自定义 <code>==</code> 操作符中判断 <code>null</code>。</a>

</div>
<div style='clear:both'></div>
