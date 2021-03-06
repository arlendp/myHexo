﻿title: javascript学习笔记

categories:
- Web开发
- JS
tags:
- Web开发
- JS

date: 2016/03/04
folder: web/js
en-title: js-study-notes
---
本文是对《javascript权威指南》这本书中的知识点的总结。
<!-- more -->

## 1. 词法结构

* js标识符必须以字母、下划线（`_`）或美元符（`$`）开始，后续字符可以是字母、数字、下划线或美元符。
* 当缺少分号时，js并不是在所有换行处填补分号，而是在缺少分号就无法正确解析代码的时候才会填补分号。但有两个例外：
    * 当`return`、`break`和`continue`语句后面紧接着换行时，js会在换行处添加分号。
    * 当涉及到`++`和`--`符号时，若运算符作为后缀使用，应和表达式在同一行，若此时换行，js会在行尾填补分号，运算符会作为下一行代码的前缀运算符。

## 2. 类型、值和变量

* 对象、数组属于可变类型，数字、字符串、布尔值、`null`和`undefined`属于不可变类型。
* 任意js的值都可以转换为布尔值。`undefined`、`null`、`0`、`-0`、`NaN`、`""`会转化成`false`，其他值都会转换成`true`。
* `null`是一种特殊的对象。`undefined`属于`undefined`类型，变量没有初始化，当查询的对象或数组的属性或元素不存在，如果函数没有返回任何值，引用没有提供实参的函数形参的值都会返回`undefined`。`undefined`是预定义的全局变量，不是关键字。`==`会认为两者相等，`.`和`[]`对两者进行操作时都会产生类型错误。若想将他们赋值给变量、属性或作为参数传入函数建议使用`null`。
* 不在任何函数内的js代码可以使用关键字`this`来引用全局对象。
* 在读取字符串、数字和布尔值的属性值时，表现的像对象一样。但如果你给其属性赋值，则会忽略这个操作：修改只是发生在临时对象身上，而这个临时对象并未被保留下来。
* 对象的比较并非值的比较：即使两个对象包含同样的属性及相同的值，他们也是不相等的。
* `x + ""`等价于`String(x)`，`+x`等价于`Number(x)`，`!!x`等价于`Boolean(x)`。
* `+`、`==`、`!=`和关系运算符是唯一执行特殊字符串到原始值的转换方式的运算符。
* 使用`var`语句重复声明变量是合法无害的。若给未声明的变量赋值，js会给全局对象创建一个同名属性。
* 函数体内，局部变量的优先级高于同名的全局变量。
* js使用了函数作用域，即在函数内声明的所有变量在函数体内始终是可见的。声明提前：js函数内声明的所有变量都（不包含赋值）被提前至函数体的顶部。
* 词法作用域：通过阅读包含变量定义在内的数行源码就能知道变量的作用域。

## 3. 表达式和运算符

* 如果表达式后跟一对方括号，则会计算方括号内的表达式的值并将它转换为字符串。只有当属性名称是合法的标识符并且需要知道要访问的属性名时，才能使用`.`来访问属性。
* 如果函数使用`return`给出一个返回值时，这个返回值就是该表达式的值，否则表达式的值就是`undefined`。
* 如果对象创建表达式不需要传入任何参数给构造函数的话，那么空圆括号是可以省略的。
* 构造函数一般不会返回一个值，并且这个新创建并被初始化后的对象就是整个对象创建表达式的值。如果该构造函数返回了一个对象值（只能是对象），那么该对象则作为整个表达式的值，而构造函数中新创建的对象就废弃了。
* js中子表达式的计算过程中的运算顺序不同于运算符的优先级和结合性规定的运算顺序。js总是严格按照从左至右的顺序来计算表达式的。如计算式

    ```
    var a = 1;
    var b = (a++) + a;
    ```

    1. 计算b
    2. 计算a++（结果记为c），之后a的值增1
    3. 计算a
    4. 计算c+a（此时a为2）
    5. 将结果赋值给b

* 所有无法转换为数字的操作数都转化为`NaN`，若有操作数为`NaN`，此时计算结果也为`NaN`。
* 求余运算中余数的符号和被除数的符号保持一致。
* 当`+`与字符串和数字一起使用时，应考虑加法的结合性对运算的影响。

    ```
    var a = 1 + 2 + "hello";//"3hello"
    var b = 1 + (2 + "hello");//"12hello"
    ```
* 位运算符的操作数为整数，且为32位整型，若不是则会强制转换。
    * 左移（<<）新的一位会用0来补充
    * 右移（>>）新的一位由操作数的符号决定，正数补0，负数补1。
    * 无符号右移（>>>）新的一位总是补0.
* 只有数字和字符串才能真正执行比较操作，除此之外的都将进行类型转换。如果一个操作数是（或转换之后是）`NaN`，则比较结果总为`false`。字符串比较中大写字母总是小于小写字母。
* in运算符：如果右侧的对象有一个名为左侧操作数的属性名，那么表达式返回`true`。
* instanceof运算符：左操作数是对象，右操作数是对象的类。若左操作数不是对象，则返回`false`，若右操作数不是函数，则抛出类型错误异常。
* `&&` 和 `||`的“短路”性质，可用于简写代码：

    ```javascript
    if (a == b) {
        stop();
    }

    (a == b) && stop();
    ```
* 带操作符的赋值运算中需要注意：表达式`a op= b`和`a = a op b`的不同点，当a中含有具有副作用的表达式时，两者是不同的。如：

    ```javascript
    var data = [1, 2 ,3];
    var i = 0;
    //data[i++] *= 2;
    //data[i++] = data[i++] * 2;
    两者计算结果不同，第一个data = [2, 2, 3]，第二个data = [4, 2, 3]
    ```
* `typeof`运算符返回表示操作数类型的字符串，可将数组和对象与函数区分开。
* `delete`运算符用来删除对象属性或数组元素，删除属性时该属性在对象中不再存在，删除数组元素时，其对应值变为`undefined`。但通过`var`声明的变量和定义的函数都不能被删除。
* `void`运算符：操作数会照常计算但是忽略计算结果并返回`undefined`。常用于url中。

## 4. 语句

* 全局变量是全局对象的属性，然而和其他全局变量的属性不同的是，var声明的变量是无法通过delete删除的。
* 函数声明语句和函数定义表达式的不同点：
    * 函数定义语句中的函数名称和函数体均被显式的提前到脚本或函数的顶部，因此他们在整个脚本和函数内都是可见的。
    * 使用var声明的函数表达式中只有变量声明被提前了，变量的初始化代码仍在原来的位置。
* `default`标签可以放在`switch`语句的任何位置，并不会影响结果。
* `while(true)`和`for(;;)`都表示死循环。

## 5. 对象

* 如果变量x是指向一个对象的引用，那么执行代码`var y = x;变量y也是指向同一个对象的引用，而非这个对象的副本。通过变量y修改这个对象同样会对变量x的值造成影响。
* 对象的属性名可以是包含空字符串在内的任意字符串，但对象中不能存在两个同名的属性。同时属性名也可以是标识符，但若属性名含有非法字符或是关键字则需要带上引号。
* 用`.`操作对象时，右侧必须是一个以属性名命名的标识符。用`[]`操作对象时，方括号内的表达式必须返回一个字符串或者可以转换为字符串的值。
* 若要查询对象o的属性x，如果o中不存在x，那么将会继续在o的原型对象中查询属性x，如果原型对象中也没有x，但这个原型对象也有原型，那么继续在这个原型对象的原型上执行查询，直到找到x或者找到一个原型是null的对象为止。
* 若给对象o的属性x赋值，如果o中已经有了属性x，那么这个赋值操作只改变这个已有属性x的值；如果o中不存在属性x，那么赋值操作给o添加了新属性x；如果之前o继承了属性x，那么这个继承的属性就被新创建的同名属性覆盖了。
* `delete`只是断开属性和宿主对象的联系，只能删除自有属性而不能删除继承属性，若一定要删除则只能从定义这个属性的原型对象上删除。（但这会影响到所有继承自这个原型的对象）
* `in`运算符可检测是否含有自有属性和继承属性，`hasOwnProperty`只能检测是否含有自由属性。
* 可通过`o.x !== undefined`判断是否含有某种属性，效果等同于in运算符。（但此种方法不能区分存在但值为undefined的属性）
* for/in循环可以遍历对象中所有可枚举的自有属性和继承属性，但不能枚举继承的内置方法。
* `Object.keys()`返回可枚举的自有属性，`Object.getOwnPropertyNames()`返回所有自有属性。
* 存取器属性`getter`和`setter`。
* `Object.getOwnPropertyDescriptor(Object, attributeName)`获得某个对象特定自有属性的属性描述符。
* `Object.defineProperty(Object, attributeName, descriptorObject)`可以设置属性的特性。
* 通过对象直接量创建的对象使用`Object.prototype`作为它的原型，通过new创建的对象使用构造函数的prototype属性作为它的原型，通过`Object.create()`创建的对象使用第一个参数作为它的原型。
* `Object.getPrototypeOf()`可以查询对象的原型。
* `ObjectA.isPrototypeOf(ObjectB)`检测对象A是否是对象B的原型。
* 对象的可扩展性表示是否可以给对象添加新属性。`Object.isExtensible()`判断对象是否可扩展；`ObjectPreventExtensions()`将对象转换为不可扩展的，此时再无法将对象转换回可扩展的了，同时这样做只能影响到对象本身的可扩展性，若给一个不可扩展的对象的原型添加属性，则该对象同样会继承这个新属性。

## 6. 数组

* js数组可以是稀疏的，数组元素的索引不一定要连续，他们之间可以有空缺，对于每一个数组都有`length`属性，针对非稀疏数组，该属性就是数组元素的个数，而对于稀疏数组，该属性值比所有元素的索引都要大。
* 数组直接量的语法允许有可选的结尾的逗号，因此`var a = [, ,]`含有两个`undefined`值而非三个。
* 数组的索引是**0~2^32-2**之间的整数。
* 因为数组是对象，因此可以为其创建任意名字的属性，但是如果属性名是数组的索引，数组就会更新它的length属性值。
* 可以使用负数或者非整数作为数组的索引，此时数值将转换为字符串来作为属性名使用，此时只能作为属性名而非数组索引。同样，如果使用了非负整数的字符串作为数组索引，它就会直接作为数组索引而非对象的属性值。当使用浮点数作为索引时，若浮点数与一个整数相等则同样方式处理。
* 关于`length`属性：
    * 如果为一个数组元素赋值，它的索引 **i** 大于或等于现有数组的长度时，length的属性值将设置为 **i+1**
    * 若设置length属性为一个小于当前长度的非负整数n时，当前数组中那些索引值大于或等于n的元素将从中删除。

## 7. 函数

* 函数的定义可以通过函数声明或者函数定义表达式。通过函数声明的方法定义的函数，其名称是必需的部分，函数声明实际上声明了一个变量并把一个函数赋值给它。通过函数定义表达式的方式定义的函数，其名称是可选的，如果它包含名称，函数的局部作用域将会包含一个绑定到该函数对象的名称，函数的名称将成为函数内部的一个局部变量，在比如函数需要递归的情况下是很有用的。
* 函数声明语句被提前到外部脚本或外部函数作用域的顶部，所以这种方式声明的函数可以被在它定义之前出现的代码所调用。要调用以表达式定义的函数则需要将函数赋值给一个变量，变量的声明会提前但是变量的赋值是不会提前的。
* 函数声明语句不能出现在循环、条件判断、或者try/catch/finally以及with语句中。函数定义表达式则不受限制。
* 普通的函数调用中（无论该函数声明是在脚本中还是函数内）的上下文`this`指的是全局对象（严格模式中是undefined），而方法调用中则指的是调用该方法的对象。
* 构造函数调用创建一个新的空对象，这个对象继承自构造函数的prototype属性，构造函数会试图初始化这个新创建的对象，并将这个对象用作其调用上下文，因此构造函数中的this指的是这个新创建的对象。
* 构造函数通常不使用`return`返回值，因为构造函数会显式返回初始化的新对象，如果使用了return返回一个对象，则调用构造函数返回的就是这个对象，若return返回的是其他值则会忽略该返回值。
* 在函数体内标识符`arguments`是指向实参对象的引用，实参对象是一个类数组对象，可以通过数字下标来访问传入函数的实参值。
* `arguments`和形参指的是同一个值，修改任意一个值都会影响到另一个。
* 函数不仅是一种语法也是值。
* 闭包的实现和理解，词法作用域。
* `call()`方法和`apply()`方法可以看作是某个对象的方法，通过调用方法的形式来间接调用函数。对于call函数来说，第一个调用上下文参数之后的所有参数都是要传入的待调用函数的实参；而apply方法则将实参都放入一个数组当中。两种方法的第一个参数都是一个要调用该函数的对象，该函数中的`this`则指向这个对象。如：

    ```javascript
    function add(y) {
        return this.x + y;
    }
    var a = {x: 1};
    add.call(a, 2)//返回3
    ```

* `bind()`方法将函数绑定至某个对象，并返回一个新的函数。如：

    ```
    function f(y) {
        return this.x + y;
    }
    var a = {x: 1};
    var g = f.bind(a);
    g(2)//返回3
    上述过程相当于
    a = {x: 1, f: function(y) {return this.x + y}};
    g = a.f;
    ```

* 通过`Function()`构造的函数不使用词法作用域，它的作用域是全局作用域。
* js在函数式编程中的使用。

## 8. 类和模块

* 定义构造函数即是定义类，因此构造函数名的首字母要大写，而普通的函数和方法名首字母都是小写。
* 构造函数不必用`return`返回值，当通过`new`关键字来创建新对象时会自动返回该对象。其原型对象的名字为`ClassName.prototype`，这是一个强制命名，通过该构造函数创建的新对象会自动使用该原型对象作为新创建的对象的原型。
* **原型对象是类的唯一标识**，当且仅当两个对象继承自同一个原型对象时，它们才是属于同一个类的实例，而初始化该对象的构造函数则不能作为类的标识，两个构造函数的prototype可能指向同一个原型对象，那么这两个构造函数创建的实例是属于同一个类的。
* `a instanceof A`并不会检查a是否是由A()构造函数初始化而来，而是检查a是否继承自A.prototype。
* 在希望用到字符串的地方用到对象的话，js会自动调用`toString`方法，如果没有实现这个方法，类会默认从`Object.prototype`中继承这个方法。
* 代码的模块化很重要，模块是一个独立的js文件，模块文件可以包含一个类定义、一组相关的类、一个实用函数库或者是一些待执行的代码。
* 所有模块都尽量定义不超过一个全局变量。
* 创建模块的过程中，避免污染全局变量的一种方法是使用一个对象作为命名空间。

## 9. 正则表达式的模式匹配

正则表达式部分在我的另一篇博客中有介绍到：[javascri正则表达式]()

## 10. web浏览器中的javascript

* 在html文档里嵌入客户端javascript代码有四种方法：
    * 内联方式，放置在`<script></script>`标签对之间。
    * 放置在`<script>`标签的src属性指定的外部文件中。
    * 放置在html事件处理程序中，如放入onclick的属性值中。
    * 放在url中，该url使用特殊的"javascript:"协议。

* 当使用`src`属性时，`<script></script>`之间的任何内容都会忽略。
* `script`标记中type属性默认值为"text/javascript"，如果没有指定，则使用默认值。若指定的类型是一个不可执行的类型，则不会从该url中下载任何东西。
* 可以通过`<a href="javascript: void doSomethingHere;">`来执行某些操作并且不会修改当前页面文档。
* 脚本和事件处理程序在同一时间只能执行一个，没有并发性。
* javascript的时间线。

## 11. Window对象

* 它是客户端js的全局对象。
* `Location`对象的`assign()`和`replace()`方法都可以使窗口载入一个指定的url中的文档，后者在载入新文档之前会将当前文档从浏览历史中删除，此时后退操作无效。
* `Location`对象的`reload()`方法可以让浏览器重新载入当前文档。
* 直接将url赋值给`location`属性。
* 片段标识符不会使浏览器载入新的文档，它只会使它滚动到文档中的某个位置。`#id`会使浏览器跳到元素id对应的位置。`#top`如果没有元素id为top的话，则浏览器会跳到文档的开始处。
* `history`属性包含浏览器的浏览历史信息。
* `navigator`属性包含浏览器厂商和版本信息。
* `screen`属性包含窗口大小和可用颜色信息。
* 浏览器会为了防止广告的弹出而禁用`window.open()`方法，只有当用户手动点击按钮或者链接的时候才会调用。
* 如果正在事件处理程序中调用`close()`方法，则应指明是Window对象还是Document对象的close方法。
* 即使一个窗口已经关闭，但是代表它的Window对象仍然存在，它的document会使null，它的方法也不会工作。
* Window对象的open()方法会返回新创建的窗口的Window对象，而该对象的opener属性则指向打开该窗口的原始窗口的Window对象。
* 窗体是用`<iframe>`元素创建的，窗体或窗口之间可以互相嵌套，可通过`parent`引用父窗口或窗体的window对象，`top`则可直接引用顶级窗口对象。若获取了iframe元素，可通过`contentWindow`属性获取该窗体的window对象，相反可通过window对象的`frameElement`得到对应的元素。另外，Window对象中还有frames属性可以得到自身包含的子窗口或窗体的引用，frames属性引用的是类数组对象，数组中的元素是Window对象而不是iframe元素，当访问子窗体时也可通过iframe元素的name或id属性来访问。

## 12. 脚本化文档

* `Document`对象表示窗口的内容。
* 在html的树形结构中，树形的根部是`Document`节点，代表整个文档，代表html元素的是`Element`节点，代表文本的是`Text`节点。这三种节点都是Node的子类。
* html的`name`属性最初是为了表单元素分配名字，在表单数据提交到服务器时使用该属性的值。name属性的值在html文档中不必唯一，并且该属性仅在表单、表单元素、iframe和img这些元素中有效。（**对于IE浏览器，通过`getElementById()`和`getElementsByName()`均会返回包含对应id和name的元素，因此不应将同样的字符串用作id和name的值**）
* `document.documentElement`指代文档的根元素，`document.head`和`document.body`分别指代head和body元素。
* `NodeList`和`HTMLCollection`都是类数组对象，因此不能直接使用Array的方法，但可以通过call和apply来间接调用。
* `querySelectorAll()`方法是通过css选择器的方式来匹配元素，但是其返回值并不是实时的，不会随着文档的变化而更新。`querySelector()`则是返回匹配的第一个元素。
* html中的属性名不区分大小写，但是js中的属性名则大小写敏感。因此html中的属性名在js中全部转换为小写，如果属性名包含不只一个单词，则除第一个单词外其余单词的首字母均大写。另外，有些html属性名在js中是保留字，对于这些属性一般是在属性名前加**html**前缀，例如，`for`转换为`htmlFor`，但对于`class`属性则例外，它转换为`className`。
* 表示html属性的值通常是字符串，但当html属性为布尔值或数字时，js中的属性也是布尔值或者数字。
* HTML5提供任意以`data-`为前缀的小写的属性名而在元素上绑定一些额外的信息。同时定义了Element对象的dataset属性，该对象的属性对应于上述含前缀的属性。
* `createElement()`和`createTextNode()`分别用于创建Element节点和Text节点。
* 如果通过`appendChild()`和`insertBefore()`方法将文档中已经存在的节点插入到文档中，那个节点将会从它当前的位置删除并在新的位置重新插入。
* `window.pageXOffset`、`window.pageYOffset`、`document.documentElement.scrollTop`、`document.documentElement.scrollLeft`都可以得到滚动条的位置信息。
* `window.innerWidth`、`window.innerHeight`、`document.documentElement.clientWidth`、`document.documentElement.clientHeight`都可以得到视口的尺寸信息。
* `getBoundingClientRect()`、`getClientRects()`可以得到一个元素的尺寸和位置。

## 13. 脚本化css

* style属性中的样式覆盖了样式表中的样式，而且文档的样式表中的样式覆盖了浏览器的默认样式。
* 对于`absolute`和`fixed`定位，可以通过left和right或top和bottom来设置长和宽，若通过含有width或height，则相应的right和bottom将失效。
* `z-index`属性只对兄弟元素应用堆叠效果。
* `z-index`属性不适用于非定位元素，但对于非定位元素，它的值为0。
* 对于定位元素，left和top属性指定了从容器边框内侧到定位元素边框外侧的距离。
* 如果css属性名包含多个连字符，在js中应将连字符移除并将每个连字符后紧接着的字母大写。如果css属性名在js中属于保留字，则应在该属性名前加上"css"前缀，如"cssFloat"。
* 通过js操作元素的style属性时，所有的值都是字符串，并且对于定位属性，其单位也要写上。

## 14. 事件处理

* `event`对象被当作参数传给事件处理函数，该对象的`type`属性确定了事件的类型，`target`属性确定了触发事件的对象。
* 当按下键盘按键重复产生字符时，在`keyup`事件之前会产生多个`keypress`事件，该事件对象指定的是产生的字符，而不是按键。
* `addEventListener()`能为同一个对象注册同一事件类型的多个处理程序函数，当对象上发生事件时，所有该事件类型的注册处理程序都会按照注册的顺序调用。
* 使用相同的参数在同一对象上多次调用`addEventListener()`是没用的，处理程序仍然只注册一次，同时重复调用也不会改变调用处理程序的顺序。
* 对于IE9之前不支持`addEventListener()`、`removeEventListener()`但支持类似的方法，`attachEvent()`和`detachEvent()`。两种方法类似但是存在以下几点不同：
    * 只有两个参数
    * 第一个参数使用了带"on"前缀的事件处理程序名字字符串。
    * 当给同一对象注册多个同一事件处理程序，事件发生时，会多次触发事件处理程序。
    * 通过这种方式注册的事件处理程序中的this指的是全局对象，而其他方式指的是目标对象。
* 事件的调用顺序：
    * 通过设置对象属性或html属性注册的处理程序优先调用。
    * 通过`addEventListener()`注册的处理程序按照它们的注册顺序调用。
    * 使用`attachEvent()`注册的处理程序可能按照任何顺序调用，代码不应依赖于调用顺序。

## 15. 脚本化http
* http请求的顺序是：先是请求方法和url，然后是请求头，最后是请求主体。

## 16. jquery类库

* this指的是原生文档参数，而不是jquery对象，若想使用jquery方法，则应该写成`$(this)`。
* jquery中使用同一个方法既当setter又当getter使用，用作setter时，这些方法会给jquery对象中的每一个元素设置值，当作为getter使用时，这些方法只会查询jquery对象中的第一个元素并给它设置值。
* `css()`方法返回的是当前样式，即计算样式，该返回值既可能来自style属性也可能来自样式表。
* `css()`方法不能获取复合样式，但是可以设置复合样式的值，其中的样式名既可以用连字符也可以用驼峰格式。
* `css()`方法在获取样式值时，会把数值转换成带有单位后缀的字符串返回，在设置样式值时，则会将数值转换成字符串并在必要时添加"px"后缀。
* `offset()`方法返回元素的绝对位置，用相对于文档的坐标来表示。`position()`方法返回相对于元素的`offsetParent()`的偏移量。
* 获取元素的尺寸可以使用以下几种方法:
    * `width()`、`height()`返回内容的宽度和高度。
    * `innerWidth()`、`innerHeight()`返回包含内边距的宽度和高度。
    * `outerWidth()`、`outerHeight()`返回包含边框的宽度和高度
    * 若对第三种方法传入参数`true`则返回的是包含元素外边距的宽度和高度。
* 如果插入的元素已经是文档的一部分，这些元素只会简单的移动而不是复制到新位置。
* jquery动画只支持数值属性。

## 17. 客户端存储

* 浏览器目前只支持存储字符串类型数据，若要存取其他类型的数据，需要手动进行编码和解码。

## 18. 多媒体和图形编程

* 通过`Image()`构造函数来创建一个图片对象，并将其`src`属性设置为相应的图片的url，这样由于图片元素并没有被添加到文档中，因此它是不可见的，但是浏览器会加载图片并将其缓存起来。当其他部分需要使用到该图片时便可直接从浏览器缓存中获取。
* 对于音频元素，可通过`new Audio("url")`来构造一个对象，但视频元素没有类似的构造函数。
* 在用`canvas`绘制图形时，当完成一条路径要绘制另一条路径前应使用`beginPath()`方法，如果没有使用该方法，那么添加的所有子路径都是处于当前路径上，使用`stroke()`和`fill()`方法时会作用在当前路径上的所有子路径。可能会导致重复绘制。
* 非零绕数原则：判断一个点是否在路径的内部。
* 每个`canvas`元素只有一个上下文对象，就算多次调用`getContext()`方法也会返回相同的上下文对象。
* 线段宽度是由`lineWidth`属性和当前坐标系变换决定的，与其他创建路径的方法无关。
* 文本对齐`textAlign`属性中，属性值`start`和`end`跟文本的方向有关，若文本是从左到右的则`start`和`left`是相同的，否则则相反。





































