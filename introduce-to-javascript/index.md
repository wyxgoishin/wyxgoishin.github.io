# JavaScript入门


本文章仅用于个人学习 JavaScript 的笔记记录，目前主要内容来自[廖雪峰的 JavaScript 教程](https://www.liaoxuefeng.com/wiki/1022910821149312)。目前主要看到了 Web 开发开头，因为当前内容已经基本满足了我学习 JS 的初心，而其他要学的其他东西也太多了（痛苦面具）。后续可能继续补充阅读 [JavaScript 高级程序设计](https://www.ituring.com.cn/book/2472) 和 [JavaScript 面向对象编程指南 ](https://book.douban.com/subject/21372235/) ，更加系统地了解  JS 这门神奇的语言。

<!--more-->

# JavaScript 教程(by [廖雪峰](https://www.liaoxuefeng.com/wiki/1022910821149312))

## 快速入门

JavaScript 代码可直接嵌入 html 代码中，其通常嵌入方式有两种：

- 单独使用 `<script>xxx</script>` 标签对，并把 js 代码写入标签对中，这个通常会放在 html 的 `<head></head>` 标签内；
- 同样使用 `<script scr="xxx.js"></script>` ，但是把 js 代码写在一个统一的文件里引入

此外由于 `<script>` 标签的 type 属性**默认为 text/javascript**，因此可以略去它。

### 基本语法

#### 语法

类似 Java，JavaScript 的语句以 `;` 结尾**但不强制**（浏览器的执行引擎会在后面自动添加），语句块使用 `{...}` 包裹，花括号内的语句有缩进，通常是 4 个空格**但不强制**。

下面是一些例子：

```javascript
var a = 1;
if(a > 2){
    console.log("a > 2");
    if(a < 4){
        console.log("a < 4");
    }
}
```

#### 注释

以 `//` 开头直到行末的字符被视为行注释，注释是给开发人员看到，JavaScript引擎会自动忽略。另一种块注释是用 `/*...*/`把多行字符包裹起来，把一大“块”视为一个注释。

下面是一些例子：

```javascript
// 这是一个行注释
/* 这是一个块注释
还是注释
注释结束*/
```

#### 大小写

JavaScript 严格区分大小写，这点需要注意。

### 数据类型和变量

#### 数据类型

##### Number

JavaScript 不区分整数和浮点数，统一用 Number 表示，以下都是合法的 Number 类型：

```javascript
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
0x4f; // 十六进制
023; // 八进制
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```

Number 可以直接做四则运算，规则和数学一致（注意**除法真的和数学一样**）：

```javascript
1 + 2; // 3
(1 + 2) * 5 / 2; // 7.5
2 / 0; // Infinity
0 / 0; // NaN
10 % 3; // 1
10.5 % 3; // 1.5
```

##### 字符串

字符串是用 `''` 或 `""` 包括起来的任意文本，其长度为 `''` 或 `""` 内的文本长度。

##### 布尔量

布尔量和布尔代数的表示完全一样，布尔量只有两个，分别为 `true` 和 `false`，支持的布尔运算包括**与、或、非**，其运算符分别为 `&&`、`||`、`!`。同时 `true` 和 `false` 在 `==` 比较运算中分别对应 `1` 和 `0`，即：

```javascript
true == 1; // true
false == 0; // true
```

##### 比较运算符

JavaScript 支持对任意数据类型做比较运算，需要注意的是在 JavaScript 中比较运算符有两个，分别是 `==` 和 `===`，他们的区别是：

- `==` 会自动转换数据类型再比较，很多时候，会得到非常诡异的结果；
- `====` 不会自动转换数据类型，如果数据类型不一致，返回`false`，如果一致，再比较。

由于 JavaScript 的这个缺陷，在比较时应该**总是使用 `===`**。

另一个例外是 `NaN` 和所有其他值都不相等，**包括它自身**，而**唯一能判断它的**只有 `isNaN()` 函数：

```javascript
NaN === NaN; // false;
isNaN(NaN); // true;
```

最后注意**浮点数的比较**，与其他语言类似，由于计算机二进制存储的特性，浮点数在运算过程中可能会产生精度丢失问题，所以比较两个浮点数应该同样采用比较差值是否小于某个阈值的方法：

```javascript
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
```

##### null 和 undefined

`null` 表示一个空值，而 `undefined` 表示未定义，这是 JavaScript 设计的初衷，因此在语义上 `undefined` 与其他语言如 Java, C++ 中的 `null` 类似。

但事实证明，区分两者的意义不大。大多数情况下，我们都应该用 `null`。`undefined` 仅仅在判断函数参数是否传递的情况下有用。

##### 数组

JavaScript 的数组用 `[xxx, yyy, ...]` 表示，需要注意的是**数组里的元素可以是任意数据类型**，比如：

```javascript
var a = [0, true, 'abc']
```

另一种创建数组的方式是通过 `Array()` 函数实现，但不建议使用。下面为举例：

```javascript
var a = new Array(1, 'true', false);
```

数组元素支持索引访问，索引从 0 开始。

##### 对象

JavaScript 的对象是一组由键 - 值组成的无序集合，比如：

```javascript
var a = {
    name: 'Bob',
    age: 10,
    tags: ['otaku', 'musician'],
    zipCode: null,
    isMale: false,
};
```

JavaScript 对象的**键都是字符串**， 值可以是任意数据类型且可以通过 `.`  运算符获取对象的属性比如：

```javascript
var b = a.zipCode;
```

##### 变量

JavaScript 中申明变量可以使用 `var` 关键字，变量名是英文字母、数字、`$` 和 `_` 的组合，且变量名不能是数字开头或者是关键字，下面是一些例子：

```javascript
var a1_ = 10;
var $_fjs = true;
var Bca = ['fs', 'sdf', 1];
```

**变量名也可以是中文，但不要给自己找麻烦**。

要打印变量值，可以使用 `console.log(x)` ，即可在控制台看到变量值。

##### strict 模式

JavaScript 在设计之初，并不强制变量申明使用 `var`，而没有使用 `var` 申明的变量即会被申明变成**全局变量**，这会导致在同一个页面的不同 JavaScript 文件中相同变量名的全局变量产生难以调试的错误。

为了修补 JavaScript 这一严重设计缺陷，ECMA 在后续规范中推出了 strict 模式，在 strict 模式下运行的JavaScript 代码，强制通过 `var` 申明变量，未使用 `var` 申明变量就使用的，将导致运行错误。可以通过在 JavaScript 代码的第一行加上 `'use strict';` 来启用 strict 模式。这是一个字符串，不支持 strict 模式的浏览器会把它当做一个字符串语句执行，支持 strict 模式的浏览器将开启 strict 模式运行 JavaScript。

不用 `var` 申明的变量会被视为全局变量，为了避免这一缺陷，所有的 JavaScript 代码都应该使用 strict 模式。

#### 字符串

如果字符串中包含 `''` 或 `""`，则按照惯例需要使用 `\` 进行转义。之前其他语言中类似的很多转义字符在 JavaScript 中同样也有，如 `\n`， `\\`。 

ASCII 码可以用 `\x##` 来表示，Unicode 码可以用 `\u####` 来表示，比如：

```javascript
'\x41'; // 完全等同于 'A'
'\u4e2d\u6587'; // 完全等同于 '中文'
```

字符串间**不支持减法**（会返回 `NaN`），但**纯数字字符串间**的减法不在此列，它们相减会像数学运算那样返回一个 Number 对象，即便二者的长度不等，比如：

```javascript
'a' - 'b'; // NaN
'10' - '9'; // 1
```

##### 多行字符串

在 `ES6` 中新增了一种表示多行字符串的方法，即用反引号 `xxx` 来表示，例如：

```javascript
var c = `你好
我是
多行字符串`;
```

##### 模板字符串

要把多个字符串连接起来，可以使用 `+` 操作符。而在 `ES6` 中，新增了一种模板字符串用于方便连接多个字符串，比如：

```javascript
var a = 'I';
var b = 'love';
var c = 'you';
var d = `${a} ${b} ${c}!`; // I love you!
```

##### 操作字符串

JavaScript 的字符串是**不可变对象**，支持通过索引查询，也可以通过索引赋值，但不会有任何效果，比如：

```javascript
var str = 'hello';
str[0]; // 'h'
str[1] = 'c';
str; // hello
```

字符串的一些常见方法如下，这些方法同样**不改变原字符串**，而是返回一个新的字符串：

```javascript
var str = 'str';
str.length;
str.toUpperCase();
str.toLowerCase();
str.indexOf(elem); // 找到一个出现该字符串的位置，没有则返回 -1
str.indexOf(elem, startIndex);
str.lastIndexOf(elem);
str.substring(a); // 从 a 开始到末尾
str.substring(a, b); // str[a, b - 1];
```

#### 数组

要获取数组的长度，可以通过 `.length` 操作获取。但请注意，**直接给 `array.length` 赋比它大的值会导致数组长度变大或变小，多出来的部分为 `undefined`**。比如：

```javascript
var a = [1, 2, 3];
a.length = 4;
a; // [1, 2, 3, undefined]
a.length = 1;
a; // [1]
```

数组支持通过索引直接更改元素值，但**如果索引大于数组长度，程序不会报错， 但会导致数组长度发生变化**。比如：

```javascript
var a = [1, 2, 3];
a[5] = 4;
a; // [1, 2, 3, undefined, undefined, 4]
```

下面介绍一些数组常用函数：

```javascript
var a = [1, 2, 3];
a.indexOf(elem);
a.indexOf(elem, startIndex);
a.lastIndexOf(elem);
a.slice(c, d); // 返回一个新的数组，对应 a[c, d - 1]，如果 c, d 范围不对也不会报错，只会返回有效部分
a.slice(c); // 返回一个新的数组，对应 a 从索引 c 开始到末尾部分
a.slice(); // 返回一个原数组的复制
a.push(*elem); // 向末尾添加元素，添加顺序和括号内顺序一致a
a.push(1, 2); // a -> [1, 2, 3, 1, 2]
a.pop(); // 从末尾删除一个元素，空数组不会报错，而是返回 undefined
a.unshift(*elem); // 向头部添加元素，添加顺序与括号内顺序相反；
a.unshift(5, 4); // a -> [5, 4, 1, 2, 3];
a.shift(); // 从头部删除一个元素，空数组不会报错，而是返回 undefined
a.sort(); // 对原数组进行排序，会修改原数组，默认排序时先把数组元素转化成字符串，因此结果很奇怪
a.reverse(); // 反转原数组
a.splice(c, d); // 删除 a 数组中索引 c 开始的 d 个元素，仅删除有效部分，同时返回删除的部分数组
a.splice(c, d, *elem); // 删除后再从该位置添加对应元素，同时返回删除的部分数组
a.splice(1, 2, 4, 5); // a -> [1, 4, 5];
a.concat(*elem, *Array); // 返回一个新数组，其元素为 a 和所有数组的连接
a.concat(4, 5, [6, 7]); // a -> [1, 2, 3, 4, 5, 6, 7]
a.join(elem); // 将 a 中元素全部用 elem 连接并返回，如果 elem 非字符串则先转为字符串
var b = [[1, 2], [3, 4], [5, 6]]; // 多维数组，索引
```

#### 对象

JavaScript 用一个 `{...}` 表示一个对象，键值对以 `xxx: xxx` 形式申明，用 `,` 隔开。注意，最后一个键值对不需要在末尾加 `,` ，如果加了，有的浏览器（如低版本的IE）将报错。

对象的属性名如果包含特殊字符，则必须用 `` 包裹起来，比如：

```javascript
var a = {`middle-school`: 'zju'};
```

如果访问对象不存在的属性，也不会报错，而是会返回 `undefined`。

由于 JavaScript 的对象是动态类型，也可以通过 `.` 操作给对象动态增加属性，比如：

```javascript
var a = {name : 'wyx'};
a.age = 18; // a -> {name: 'wyx', age: 18};
```

如果要判断对象是否拥有某个属性，则可以使用 `in` 操作符，但该方法对于继承的属性同样会返回 `true`，因为 JavaScript 的对象在原形链上最终都指向 `Object` 对象。如果想判断是否是对象自身所有的，则可以选择采用 `hasOwnProperty` 方法，比如：

```javascript
var a = {name : 'wyx'};
'toString' in a; // true, toString 是 Object 对象的属性
a.hasOwnProperty('toString'); // false
```

#### 条件

JavaScript 使用 `if{...}else{...}` 进行条件判断，注意对于 `if` 或 `else` 后的语句使用花括号进行包裹，否则对于多行语句，即便有缩进也不会被包在对应语句下，比如：

```javascript
var a = 10;
if (a > 5){
    console.log(a);
}else
    console.log(10);
	console.log(20); // 这行语句永远都会被输出
```

对于多行条件判断，也可以采用多个 `if{...}else{...}` 的组合，即类似 `else if{....}` 的感觉。

需要注意在**条件判断语句中** JavaScript **把 `null`、`undefined`、`0`、`NaN` 和 空字符串 `''` 视为 `false`，其他值一概视为 `true`**。

ES6 中也支持 `?` 操作符，即 `expression ? expression : expression`。

#### 循环

类似 Java，JavaScript 支持使用 `for(expr; expr; expr)` 形式进行循环，比如：

```javascript
for(var a = 1; a < 10; a++){
    console.log(a);
}
```

同时对于对象或数组，JavaScript 还支持通过 `for ... in` 把对象的属性或数组元素循环出来：

```javascript
var a = {name: 'wyx', age: 10, gender: false};
for(var key in a){
    console.log(key);
}
var b = [1, '2', null];
for(var elem in b){
    console.log(elem);
}
```

此外， JavaScript 还支持 `while` 循环和 `do ... while ...` 循环。

#### Map 和 Set

先前介绍的 JavaScript 的对象可以视为其他语言中的 Map 或者 Dictionary，但是 JavaScript 中对象的属性只能是字符串，这便带来许多不便。因此在 ES6 中，引入了新的数据类型 Map。

##### Map

要定义一个 Map，可以通过 `new` 关键字，比如：

```javascript
var map1 = new Map(); // 初始化空的 Map
var map2 = new Map([[key1, value1], [key2, value2], ...]);
```

Map 主要有以下常用方法：

```javascript
map.set(key, value);
map.get(key); // 若不存在则返回 undefined
map.has(key);
map.delete(key); // 返回 true 或 false， 结果视是否删除成功而定
```

##### Set

要定义一个 Set，可以通过 `new` 关键字，比如：

```javascript
var set1 = new Set(); // 初始化空的 Set
var set2 = new Set([elem1, elem2, ...]);
```

Set 主要有以下常用方法：

```javascript
set.add(elem);
set.has(elem);
set.delete(elem); // 返回 true 或 false, 结果视是否删除成功而定
```

#### Iterable

遍历 Array 可以采用下标循环，遍历 Map 和 Set 就无法使用下标。为了统一集合类型，ES6 标准引入了新的iterable 类型，Array、Map 和 Set 都属于 iterable 类型。

具有 iterable 类型的集合可以通过新的 `for ... of` 循环来遍历，注意这个是 ES6 新引入的特性。其中 Map 遍历时返回的是键值对，下面是示例：

```javascript
var map1 = new Map([[1, 2], [3, 4]]);
for(var entry of map1){
    console.log(entry);
}
```

`for ... in` 循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个 Array 数组实际上也是一个对象，它的每个元素的索引被视为一个属性。因此如果你手动给它添加额外的属性，则会发生奇怪的错误，比如：

```javascript
var a = [1, 2, 3];
a.name = 'array';
for(var entry in a){
    console.log(entry); // 1, 2, 3, array
}
```

而 `for ... of` 则修复了这个错误，只输出数组本身的元素。

然而，更好的方式是直接使用`iterable`内置的`forEach`方法，它接收一个函数，每次迭代就自动回调该函数，以遍历 Array 为例：

```javascript
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});
```

**注意**，`forEach()`方法是 ES5.1 标准引入的，你需要测试浏览器是否支持。

`Set` 与 `Array`类似，但 `Set` 没有索引，因此回调函数的前两个参数都是元素本身：

```javascript
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);
});
```

`Map` 的回调函数参数依次为 `value`、`key` 和 `map` 本身：

```javascript
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```

如果对某些参数不感兴趣，由于 JavaScript 的函数调用不要求参数必须一致，因此可以忽略它们。例如，只需要获得`Array`的`element`：

```javascript
var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    console.log(element);
});
```

### 函数

#### 函数的定义和调用

##### 定义函数

在 JavaScript 中，可以通过 `function` 关键字定义函数，其形式如下：

```javascript
function func_name(param1, param2, ...){
    // function_body;
};
function abs(x){
    return x > 0 ? x : -x;
};
```

由于 **JavaScript 的函数也是一个对象**，因此函数也可以这么定义：

```javascript
var func = function(param1, param2, ...){/*function_content*/};
```

另外 JavaScript 的函数支持嵌套，比如：

```javascript
function external_func(*param){
    // function_body
    function internal_func(*param){
        // function_body
    }
};
```

##### 调用函数

由于 **JavaScript 允许传入任意个参数而不影响调用**，因此当你传入多余的参数时并不会报错，传入参数过少时 JavaScript 会将缺少的部分标记为 `undefined` 并返回 `NaN` 而不会报错。

要判断某个参数的类型，可以使用 `typeof`，比如：

```javascript
var a = 10;
typeof a === 'number'; // true;
```

##### arguments

JavaScript 还有一个免费赠送的关键字 arguments，它只在函数内部起作用，并且**永远指向当前函数的调用者传入的所有参数**。arguments 类似 Array 但它不是一个 Array：

```javascript
function foo(x) {
    console.log('x = ' + x); // 10
    for (var i=0; i<arguments.length; i++) {
        console.log('arg ' + i + ' = ' + arguments[i]); // 10, 20, 30
    }
};
foo(10, 20, 30);
```

而实际上 `arguments` 最常用于判断传入参数的个数。

##### rest 参数

ES6 标准引入了 rest 参数，它**必须用 `...rest` 写在函数参数最后**，多余部分的参数会以**数组**形式传给 rest，没有则 rest 为 空数组，比如：

```javascript
function func1(a, b, ...rest){/*function_body*/}
function func2(...rest){/*function_body*/}
```

##### return 语句的坑

前面说到 JavaScript 不要求语句末尾加 `;`，当缺失时会自动在末尾加上，所以对于某些 return 语句就会发生奇怪的错误，比如：

```javascript
function func1(){
    return // 会变成 return; 导致第 3 行代码被忽略
    	{name: 'foo'};
}
```

#### 变量和作用域

- 函数体内声明的变量在仅函数体内**任意地方**均有用，反之则具有全局作用域；

```java
function foo(){
    for(var i = 1; ...; ...){
        ...;
    }
    console.log(i); // 正确，不会报错
}
```

- 内部函数体可以访问外部函数体的变量，反过来则不行；
- 当内部函数体的变量和外部函数体的变量重名时，则会使用内部函数体定义的变量。

##### 变量提升

JavaScript 的函数定义有个特点，它会先扫描整个函数体的语句，把所有 `var` 申明的变量“提升”到函数顶部：

```javascript
'use strict';

function foo() {
    var x = 'Hello, ' + y; // Hello, undefined
    console.log(x);
    var y = 'Bob';
}

foo();
```

上述代码第 4 行不会报错，但输出 `Hello, undefined`，说明 JavaScript 提前了变量的声明，但变量的赋值则没有。实质上上述代码中的函数定义在 JavaScript 引擎看来类似：

```javascript
function foo(){
    var y; // undefined;
    var x = 'Hello, ' + y;
    console.log(x);
    y = 'Bob';
}
```

因此在函数体中，我们需要严格遵守先提前申明变量这一准则，最常见的做法是用一个 `var` 声明所有函数体内的变量：

```javascript
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // function_body
}
```

##### 全局作用域

JavaScript 默认有一个全局对象 `window`，全局作用域的变量实际上被绑定到 `window` 的一个属性：

```javascript
var a = 'js';
a in window; // false
window.a; // 'js'
```

顶层函数的定义也被视为一个全局变量，并绑定到`window`对象：

```javascript
function func1(*params){
    // function_body
}
window.func1(); // 完全没问题
```

进而大胆猜测，`alert` 函数其实也是 `window` 的一个属性；

这说明 JavaScript 实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报`ReferenceError`错误。

##### 名字空间

全局变量会绑定到 `window`上，不同的 JavaScript 文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。

减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：

```javascript
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```

把自己的代码全部放入唯一的名字空间`MYAPP`中，会大大减少全局变量冲突的可能。

许多著名的 JavaScript 库都是这么干的：jQuery，YUI，underscore等等。

##### 局部作用域

由于 JavaScript 的变量作用域实际上是函数内部，我们在 `for` 循环等语句块中是无法定义具有局部作用域的变量的：

```javascript
'use strict';

function foo() {
    for (var i=0; i<100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
}
```

为了解决块级作用域，ES6 引入了新的关键字 `let`，用 `let` 替代 `var` 可以申明一个块级作用域的变量：

```javascript
'use strict';

function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

##### 常量

ES6 中引入了 `const` 关键字来定义常量， `const` 和 `let` 都具有块级作用域：

```javascript
'use strict';

const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
```

##### 解构赋值

从 ES6 开始，JavaScript 引入了解构赋值，可以同时对一组变量进行赋值，举个例子：

```javascript
var [x, y, z] = ['hello', 'JavaScript', 'ES6']; // x -> hello, y -> JavaScript, z -> ES6
```

注意，对**数组元素进行解构赋值时，多个变量要用`[...]`括起来**。

如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致：

```javascript
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
x; // 'hello'
y; // 'JavaScript'
z; // 'ES6'
```

解构赋值还可以忽略某些元素：

```javascript
let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对 z 赋值第三个元素
z; // 'ES6'
```

对一个对象进行解构赋值时，同样可以直接对**嵌套的对象属性**进行赋值，只要保证对应的层次是一致的：

```javascript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
name; // '小明'
city; // 'Beijing'
zip; // undefined, 因为属性名是zipcode而不是zip
// 注意: address不是变量，而是为了让city和zip获得嵌套的address对象的属性:
address; // Uncaught ReferenceError: address is not defined
```

使用解构赋值对对象属性进行赋值时，如果对应的属性不存在，变量将被赋值为`undefined`，这和引用一个不存在的属性获得`undefined`是一致的。如果要使用的变量名和属性名不一致，可以用下面的语法获取：

```java
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};

// 把passport属性赋值给变量id:
let {name, passport:id} = person;
name; // '小明'
id; // 'G-12345678'
// 注意: passport不是变量，而是为了让变量id获得passport属性:
passport; // Uncaught ReferenceError: passport is not defined
```

解构赋值还可以使用默认值，这样就避免了不存在的属性返回`undefined`的问题：

```java
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678'
};

// 如果person对象没有single属性，默认赋值为true:
var {name, single=true} = person;
name; // '小明'
single; // true
```

有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：

```javascript
// 声明变量:
var x, y;
// 解构赋值:
{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
```

这是因为 JavaScript 引擎把`{`开头的语句当作了块处理，于是`=`不再合法。解决方法是用小括号括起来：

```javascript
({x, y} = { name: '小明', x: 100, y: 200});
```

###### 使用场景

解构赋值在很多时候可以大大简化代码。例如，交换两个变量`x`和`y`的值，可以这么写，不再需要临时变量：

```javascript
var x=1, y=2;
[x, y] = [y, x]
```

快速获取当前页面的域名和路径：

```javascript
var {hostname:domain, pathname:path} = location;
```

如果一个函数接收一个对象作为参数，那么，可以使用解构直接把对象的属性绑定到变量中。例如，下面的函数可以快速创建一个`Date`对象：

```javascript
function buildDate({year, month, day, hour=0, minute=0, second=0}) {
    return new Date(year + '-' + month + '-' + day + ' ' + hour + ':' + minute + ':' + second);
}
```

它的方便之处在于传入的对象只需要`year`、`month`和`day`这三个属性：

```javascript
buildDate({ year: 2017, month: 1, day: 1 });
// Sun Jan 01 2017 00:00:00 GMT+0800 (CST)
```

也可以传入`hour`、`minute`和`second`属性：

```javascript
buildDate({ year: 2017, month: 1, day: 1, hour: 20, minute: 15 });
// Sun Jan 01 2017 20:15:00 GMT+0800 (CST)
```

使用解构赋值可以减少代码量，但是，需要在支持 ES6 解构赋值特性的现代浏览器中才能正常运行。目前支持解构赋值的浏览器包括 Chrome，Firefox，Edge等。

#### 方法和 this

绑定到对象上的函数称为方法，需要注意的该方法中可以使用 `this` 关键字指向**当前对象**，比如：

```javascript
var obj = {
    name: 'wyx',
    age: 18,
    intro: function(){
        return this.name + this.age;
    }
};
obj.intro(); // wyx18
```

但需要注意的是，如果某个对象的函数的定义在对象体外，同时该函数被直接调用， 则此时的 `this` 指向 `window` 对象，比如：

```javascript
function intro(){
    return this.name1 + this.age1;
};

var obj = {
    name1: 'wyx',
    age1: 18,
    introduction: intro
};
obj.introduction(); // wyx18
intro(); // NaN
```

同样直接获取对象的函数属性并调用也是不行的，比如：

```javascript
var fn = obj.introduction;
fn(); // NaN
```

要保证`this`指向正确，必须用`obj.xxx()`的形式调用！

由于这是一个巨大的设计错误，ECMA 决定，在 strict 模式下让函数的`this`指向`undefined`，因此，在 strict 模式下，你会得到一个错误：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

var fn = xiaoming.age;
fn(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

这个决定只是让错误及时暴露出来，并没有解决`this`应该指向的正确位置。

有些时候，喜欢重构的你把方法重构了一下：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

结果又报错了！原因是`this`指针只在`age`方法的函数内指向`xiaoming`，在函数内部定义的函数，`this`又指向`undefined`了！（在非 strict 模式下，它重新指向全局对象`window`！）

修复的办法也不是没有，我们用一个`that`变量首先捕获`this`：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
};

xiaoming.age(); // 25
```

用`var that = this;`，你就可以放心地在方法内部定义其他函数，而不是把所有语句都堆到一个方法中。

##### this 参数绑定解惑

this这个 keyword 非常的困惑,但是其实有一个好方法可以理解。

1. 检查 `.` 左边是谁 invoke 这个函数. 例如 xiaoming.age(); age 函数里面有 this, 然后 `.` 旁边是 xiaoming , 那么 this 就是指向 xiaoming了。这种叫做 Implicit Binding；

2. 如果点旁边没有,那就检查有没有用到 bind, apply, call 这三种, 有的话就是调用此方法的对象. 这种叫做 explicit binding；

3. 如果上面两个都没有就检查代码里面有没有用到 new 这个 keyword, 有的话那就是指向 new 旁边的函数对象。这种叫做 new binding。

4. 上面三个都没有, 检查是不是有 arrow function, 有 arrow function 的话就是, 那么指向是 arrow function 的 lexical binding 的对象. 就是它的 parent. 这种叫做 lexical binding；

5. 全部都没有如果不是 strict mode 那就是 window 对象了。 strict 就是 error (undefined).

##### apply

虽然在一个独立的函数调用中，根据是否是strict模式，`this`指向`undefined`或`window`，不过，我们还是可以控制`this`的指向的！

要指定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数。

用`apply`修复`getAge()`调用：

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

另一个与`apply()`类似的方法是`call()`，唯一区别是：

- `apply()`把参数打包成`Array`再传入；
- `call()`把参数按顺序传入。

比如调用`Math.max(3, 5, 4)`，分别用`apply()`和`call()`实现如下：

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

对普通函数调用，我们通常把`this`绑定为`null`。

##### 装饰器

利用`apply()`，我们还可以动态改变函数的行为。

JavaScript的所有对象都是动态的，即使内置的函数，我们也可以重新指向新的函数。

现在假定我们想统计一下代码一共调用了多少次`parseInt()`，可以把所有的调用都找出来，然后手动加上`count += 1`，不过这样做太傻了。最佳方案是用我们自己的函数替换掉默认的`parseInt()`：

```javascript
'use strict';

var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};
// 测试:
parseInt('10');
parseInt('20');
parseInt('30');
console.log('count = ' + count); // 3
```

#### 高阶函数

能接收其他函数作为参数的函数被称为高阶函数，比如：

```javascript
function abs(x){
    if(x > 0){
        return x;
    }else{
        return -x;
    }
};
// absAdd 即为高阶函数
function absAdd(x, y, f){
    return f(x) + f(y);
};
```

##### map、reduce、filter

`map()` 函数定义在 JavaScript 的 Array 中，其含义类似 Python 中的 map，其返回值为一个新的 Array:

```javascript
function pow(x){
    return x * x;
}
var a = [1, 2];
a.map(pow); // [1, 4]
```

另外注意 `map()` 函数接受的回调函数可以有 3 个参数：`callback(currentValue, index, array)`，在执行某些函数而**后续参数无法被忽视时**就会发生错误，比如：

```javascript
var a = ['1', '2', '3'];
a.map(parseInt); // [1, NaN, NaN]
// parseInt(string, radix) 行 2 调用时将 index 作为 radix 传入而导致后续错误
```

`reduce()` 函数同样定义在 JavaScript 的 Array 中，其含义类似 Python 的 reduce，返回一个新的值：

```javascript
function add(x, y){
    return x + y;
}
var a = [1, 2, 3];
a.reduce(add); // 1 + 2 + 3 = 6
```

`filter()` 函数同样定义在 JavaScript 的 Array 中，其含义类似 Python 的 filter，根据是否符合函数条件返回一个新的数组：

```javascript
function isOdd(x){
    return x % 2 === 1;
}
var a = [1, 2, 3];
a.filter(isOdd); // [1, 3]
```

与 `map()` 相同，`filter()` 接受的回调函数同样有 3 个参数：`callback(currentValue, index, array)`。 

##### sort

JavaScript 中 Array 的 `sort()` 函数在排序前**默认把所有元素先转换成 String** 再排序，这就会导致对数字数组进行排序时出现奇怪的结果：

```javascript
[1, 2, 10, 20].sort(); // [1, 10, 2, 20]
```

幸运的是，`sort()` 方法同样支持接收一个比较函数来实现自定义排序：

```javascript
arr.sort(function(x, y){
    // return -1, 1, 0
})
```

##### Array 的其他内置高阶函数

###### every

`every()` 方法可以判断数组中的所有元素是否满足测试条件，下面是示例：

```javascript
var a = [1, 2, 3];
a.every(function(x){return x < 4;}); // true
a.every(function(x){return x < 2;}); // false
```

###### find

`find()` 方法用于查找符合条件的第一个元素（不存在时返回 `undefined`），下面是示例：

```javascript
var a = [1, 2, 3];
a.find(function(x){return x % 2 == 0;}); // 2
a.find(function(x){return x > 3;}); // undefined
```

###### findIndex

`findIndex()` 方法用于查找符合条件的第一个元素的索引（不存在时返回 -1），下面是示例：

```javascript
var a = [1, 2, 3];
a.findIndex(function(x){return x % 2 == 0;}); // 1;
a.findIndex(function(x){return x > 3;}); // -1
```

#### 闭包

高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回，类似 Python 的 yield，比如：

```javascript
function lazy_sum(arr){
    function sum(arr){
        return arr.map(function(x, y){return x + y;});
    }
    return sum;
}
var arr = [1, 2, 3];
var f1 = lazy_sum(arr);
```

此时获得的 `f1` 并不是结果而是一个函数，只有通过调用 `f1()` 才能真正计算结果：

```javascript
f1(); // 6;
```

在这个例子中，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，**相关参数和变量都保存在返回的函数中**，这种称为“闭包（Closure）”的程序结构拥有极大的威力。

请再注意一点，当我们调用`lazy_sum()`时，每次调用都会返回一个新的函数，即使传入相同的参数：

```java
var f2 = lazy_sum(arr);
f1 === f2; // false
```

`f1()`和`f2()`的调用结果互不影响。

注意到返回的函数在其定义内部引用了局部变量`arr`，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。

另一个需要注意的问题是，返回的函数并没有立刻执行，而是直到调用了`f()`才执行。我们来看一个例子：

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count(); // arr -> [(i) -> i * i, (i) -> (i * i), (i) -> (i * i)]， i = 4
var f1 = results[0]; // arr -> [16, (i) -> (i * i), (i) -> (i * i)]
var f2 = results[1]; // arr -> [16, 16, (i) -> (i * i)]
var f3 = results[2]; // arr -> [16, 16, 16]
```

在上面的例子中，每次循环，都创建了一个新的函数，然后，把创建的 3 个函数都添加到一个`Array`中返回了。

你可能认为调用`f1()`，`f2()`和`f3()`结果应该是`1`，`4`，`9`，但实际结果是：

```javascript
f1(); // 16
f2(); // 16
f3(); // 16
```

全部都是`16`！原因就在于返回的函数引用了变量`i`，但它并非立刻执行。等到 3 个函数都返回时，它们所引用的变量`i`已经变成了`4`，因此最终结果为`16`。

返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变：

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];

f1(); // 1
f2(); // 4
f3(); // 9
```

注意这里用了一个“创建一个匿名函数并立刻执行”的语法：

```javascript
(function (x) {
    return x * x;
})(3); // 9
```

理论上讲，创建一个匿名函数并立刻执行可以这么写：

```javascript
function (x) { return x * x } (3);
```

但是由于 JavaScript 语法解析的问题，会报 SyntaxError 错误，因此需要用括号把整个函数定义括起来：

```javascript
(function (x) { return x * x }) (3);
```

通常，一个立即执行的匿名函数可以把函数体拆开，一般这么写：

```javascript
(function (x) {
    return x * x;
})(3);
```

说了这么多，难道闭包就是为了返回一个函数然后延迟执行吗？

当然不是！闭包有非常强大的功能。举个栗子：

在面向对象的程序设计语言里，比如 Java 和 C++，要在对象内部封装一个私有变量，可以用`private`修饰一个成员变量。

在没有`class`机制，只有函数的语言里，借助闭包，同样可以封装一个私有变量。我们用 JavaScript 创建一个计数器：

```javascript
function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
```

它用起来像这样：

```javascript
var c1 = create_counter();
c1.inc(); // 1
c1.inc(); // 2
c1.inc(); // 3

var c2 = create_counter(10);
c2.inc(); // 11
c2.inc(); // 12
c2.inc(); // 13
```

在返回的对象中，实现了一个闭包，该闭包携带了局部变量`x`，并且，从外部代码根本无法访问到变量`x`。换句话说，闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来。

闭包还可以把多参数的函数变成单参数的函数。例如，要计算 $x^y$ 可以用`Math.pow(x, y)`函数，不过考虑到经常计算 $x^2$ 或 $x^3$，我们可以利用闭包创建新的函数`pow2`和`pow3`：

```javascript
function myPow(n){
    return function(n){
        return Math.pow(x, n);
    }
}
var pow2 = myPow(2);
var pow3 = myPow(3);
pow2(3); // 9
pow3(5); // 125
```

##### 脑洞大开

很久很久以前，有个叫阿隆佐·邱奇的帅哥，发现只需要用函数，就可以用计算机实现运算，而不需要`0`、`1`、`2`、`3`这些数字和`+`、`-`、`*`、`/`这些符号。

JavaScript 支持函数，所以可以用 JavaScript 用函数来写这些计算。来试试：

```javascript
// 定义数字0:
var zero = function (f) {
    return function (x) {
        return x;
    }
};

// 定义数字1:
var one = function (f) {
    return function (x) {
        return f(x);
    }
};

// 定义加法:
function add(n, m) {
    return function (f) {
        return function (x) {
            return m(f)(n(f)(x));
        }
    }
}

// 计算数字2 = 1 + 1:
var two = add(one, one);

// 计算数字3 = 1 + 2:
var three = add(one, two);

// 计算数字5 = 2 + 3:
var five = add(two, three);

// 给3传一个函数,会打印3次:
(three(function () {
    console.log('print 3 times');
}))();

// 给5传一个函数,会打印5次:
(five(function () {
    console.log('print 5 times');
}))();
```

以上简易证明：

```javascript
zero(f)(x) = x;
one(f)(x) = f(x);
two(f)(x) = one(f)(one(f)(x)) = one(f)(f(x)) = f(f(x));
three(f)(x) = two(f)(one(f)(x)) = two(f)(f(x)) = f(f(f(x)));
...
five(f)(x) = f(f(f(f(f(x)))));
```

#### 箭头函数

ES6 标准中新增了一种新的箭头函数，类似但不是匿名函数，它的表示如下：

```javascript
var fn = x => x * x;
var fm = x => {/*expression*/ return xxx;}; // 这种写法必须有 return
```

如果参数不止一个或没有参数，则需要用 `()` 把参数扩起来，比如：

```javascript
var fn = () => 3.14;
var fm = (x, y) => x - y;
```

如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错：

```javascript
// SyntaxError:
x => { foo: x }
```

因为和函数体的`{ ... }`有语法冲突，所以要改为：

```javascript
// ok:
x => ({ foo: x })
```

##### this

箭头函数看上去是匿名函数的一种简写，但实际上，箭头函数和匿名函数有个明显的区别：箭头函数内部的`this`是词法作用域，由上下文确定，它**总是指向外层调用函数**。

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 25
var func = obj.getAge;
func(); // NaN
```

由于`this`在箭头函数中已经按照词法作用域绑定了，所以，用`call()`或者`apply()`调用箭头函数时，无法对`this`进行绑定，即传入的第一个参数被忽略：

```javascript
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2015); // 25
```

##### 一个有趣的例子

```javascript
name = "window";
var obj = {
  name: "obj",
  func0: () => {
    console.log(this.name);
    return () => console.log(this.name);
  },
  func1: function() {
    console.log(this.name);
    return function() { console.log(this.name); };
  },
  func2: () => {
    console.log(this.name);
    return function() { console.log(this.name); };
  },
  func3: function() {
    console.log(this.name);
    return () => console.log(this.name);
  }
}
var env = {
  name: "env",
  func0: obj.func0(), // window
  func1: obj.func1(), // obj
  func2: obj.func2(), // window
  func3: obj.func3()  // obj
}
env.func0(); // window
env.func1(); // env
env.func2(); // env
env.func3(); // obj
```

一种理解：

```javascript
func0: obj.func0(), // window          箭头函数 实际在obj中 但按照箭头函数的this规则 应该是上一层函数 虽然在var对象里 但其实属于window环境
func1: obj.func1(), // obj             匿名函数 调用环境为obj 因此为obj
func2: obj.func2(), // window          箭头函数 同func0
func3: obj.func3()  // obj             匿名函数 同func1
  
env.func0(); // window                 箭头函数 实际在obj中的箭头函数中 因此为obj中箭头函数的this（window）
env.func1(); // env                    匿名函数 调用为env
env.func2(); // env                    匿名函数 调用为env
env.func3(); // obj                    箭头函数 调用为obj中的匿名函数 此时this为obj
```

如果是箭头函数就得看它在哪，如果是匿名函数就看它的实际调用环境。

#### generator

generator 是 ES6 中新引入的数据类型，它类似 Python 的 generator，定义它需要使用 `function*`，比如：

```java
function* foo(x){
    var cur = 0;
    while(cur < x){
        yield cur;
        ++cur;
    }
    return cur;
}
```

直接调用 generator 只会返回 generator，而通过 `next()` 方法则会返回一个对象，里面包含了返回值和当前 generator 是否执行完成的 `done` 属性，比如：

```javascript
var y = foo(10);
y.next(); // {value: 0, done: false}
```

当 `done` 属性为 `false` 时，返回 `yield` 语句的值；反之则根据是否定义了 `return` 语句而返回 `undefined` 或者 `return` 语句的值。

此外，还可以通过 `for ... of` 直接迭代遍历生成器，它会遍历直到 `done` 为 `true` 为止，比如：

```javascript
var arr = [];
for(let i of foo(2)){
    arr.push(i);
}
arr; // [0, 1]
```

generator 可以用来实现保存对象的状态，也把异步回调代码变成“同步”代码。

### 标准对象

在 JavaScript 的世界里，一切都是对象。为了区分对象的类型，我们用`typeof`操作符获取对象的类型，它总是返回一个字符串：

```javascript
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.abs; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```

要特别注意 `null` ，`Array` 和对象的类型都是 `object`。

##### 包装对象

类似 Java， JavaScript 同样也提供包装对象，他们分别是 `Number`，`Boolean` 和 `String`，其类型是 `object`，使用 `new` 关键字创建：

```javascript
var n = new Number(123); // 123,生成了新的包装类型
var b = new Boolean(true); // true,生成了新的包装类型
var s = new String('str'); // 'str',生成了新的包装类型
```

如果我们在使用`Number`、`Boolean`和`String`时，没有写 `new` 关键字，这些则会被视为普通函数，并分别把任何类型的数据转换为`number`、`boolean`和`string`类型：

```javascript
var n = Number('123'); // 123，相当于parseInt()或parseFloat()
typeof n; // 'number'

var b = Boolean('true'); // true
typeof b; // 'boolean'

var b2 = Boolean('false'); // true! 'false'字符串转换结果为true！因为它是非空字符串！
var b3 = Boolean(''); // false

var s = String(123.45); // '123.45'
typeof s; // 'string'
```

所以总结一下，有这么几条规则需要遵守：

- 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
- 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
- 用`String()`来转换任意类型到`string`，或者直接调用某个对象的`toString()`方法；
- 通常不必把任意类型转换为`boolean`再判断，因为可以直接写`if (myVar) {...}`；
- `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`；
- 判断`Array`要使用`Array.isArray(arr)`；
- 判断`null`请使用`myVar === null`；
- 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`；
- 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`。

另外注意，`null` 和 `undefined` 没有 toString 方法，同时 number 调用 toString 方法会有可能报错，此时需要特殊处理：

```javascript
123.toString(); // SyntaxError
123.1.toString(); // '123.1'
123..toString(); // '123', 注意是两个点，因为 123. 也是合法的 number
(123).toString(); // '123'
```

#### Date

在 JavaScript 中，`Date`对象用来表示日期和时间。

要获取系统当前时间，用：

```javascript
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

注意，当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值。

如果要创建一个指定日期和时间的`Date`对象，可以用：

```javascript
var d = new Date(2015, 5, 19, 20, 15, 30, 123);
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)
```

你可能观察到了一个**非常非常坑爹**的地方，就是 JavaScript 的月份范围用整数表示是 0~11，`0`表示一月，`1`表示二月……，所以要表示 6 月，我们传入的是`5`！这绝对是 JavaScript 的设计者当时脑抽了一下，但是现在要修复已经不可能了。

>  JavaScript 的 Date 对象月份值从 0 开始，牢记 0=1月，1=2月，2=3月，……，11=12月。

第二种创建一个指定日期和时间的方法是解析一个符合[ISO 8601](http://www.w3.org/TR/NOTE-datetime)格式的字符串：

```javascript
var d = Date.parse('2015-06-24T19:49:22.875+08:00');
d; // 1435146562875
```

但它返回的不是`Date`对象，而是一个时间戳。不过有时间戳就可以很容易地把它转换为一个`Date`：

```javascript
var d = new Date(1435146562875);
d; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
d.getMonth(); // 5
```

> 使用Date.parse()时传入的字符串使用实际月份 1-12，转换为Date对象后getMonth()获取的月份值为 0-11。

##### 时区

`Date`对象表示的时间总是按浏览器所在时区显示的，不过我们既可以显示本地时间，也可以显示调整后的UTC时间：

```javascript
var d = new Date(1435146562875);
d.toLocaleString(); // '2015/6/24 下午7:49:22'，本地时间（北京时区+8:00），显示的字符串与操作系统设定的格式有关
d.toUTCString(); // 'Wed, 24 Jun 2015 11:49:22 GMT'，UTC时间，与本地时间相差8小时
```

那么在 JavaScript 中如何进行时区转换呢？实际上，只要我们传递的是一个`number`类型的时间戳，我们就不用关心时区转换。任何浏览器都可以把一个时间戳正确转换为本地时间。要获取当前时间戳，可以用：

```javascript
if (Date.now) {
    console.log(Date.now()); // 老版本IE没有now()方法
} else {
    console.log(new Date().getTime());
}
```

#### RegExp

JavaScript有两种方式创建一个正则表达式：第一种方式是直接通过`/正则表达式/`写出来，第二种方式是通过`new RegExp('正则表达式')`创建一个RegExp对象，比如：

```javascript
var re1 = /ABC\-001/;
var re2 = new RegExp('ABC\\-001');

re1; // /ABC\-001/
re2; // /ABC\-001/
```

注意，如果使用第二种写法，因为字符串的转义问题，字符串的两个`\\`实际上是一个`\`。

而后使用 `test()` 方法查看是否匹配成功：

```javascript
var re = /^\d{3}\-\d{3,8}$/;
re.test('010-12345'); // true
re.test('010-1234x'); // false
re.test('010 12345'); // false
```

另外要提取正则匹配的成果，可以用 `()` 将要提取部分包起来，然后使用 `exec` 方法即可提取出来，比如：

```javascript
var re = /^(\w+)@.*$/;
re.exec("wyxgoishin@gmail.com"); 
```

类似 Python，exec 如果匹配成功，则返回一个 Array，第一个永远是原字符串，第二个则是括号部分；失败则返回 null。

类似 Python，正则匹配默认为贪婪匹配，通过加上 ? 可以实现非贪婪匹配。

##### 全局搜索

JavaScript的正则表达式还有几个特殊的标志，最常用的是`g`，表示全局匹配：

```javascript
var r1 = /test/g;
// 等价于:
var r2 = new RegExp('test', 'g');
```

全局匹配可以多次执行`exec()`方法来搜索一个匹配的字符串。当我们指定`g`标志后，每次运行`exec()`，正则表达式本身会更新`lastIndex`属性，表示上次匹配到的最后索引：

```javascript
var s = 'JavaScript, VBScript, JScript and ECMAScript';
var re=/[a-zA-Z]+Script/g;

// 使用全局匹配:
re.exec(s); // ['JavaScript']
re.lastIndex; // 10

re.exec(s); // ['VBScript']
re.lastIndex; // 20

re.exec(s); // ['JScript']
re.lastIndex; // 29

re.exec(s); // ['ECMAScript']
re.lastIndex; // 44

re.exec(s); // null，直到结束仍没有匹配到
```

全局匹配类似搜索，因此不能使用`/^...$/`，那样只会最多匹配一次。

正则表达式还可以指定`i`标志，表示忽略大小写，`m`标志，表示执行多行匹配。

#### Json

道格拉斯长期担任雅虎的高级架构师，自然钟情于 JavaScript。他设计的 JSON 实际上是 JavaScript 的一个子集。在 JSON 中，一共就这么几种数据类型：

- number：和 JavaScript 的`number`完全一致；
- boolean：就是 JavaScript 的`true`或`false`；
- string：就是 JavaScript 的`string`；
- null：就是 JavaScript 的`null`；
- array：就是 JavaScript 的`Array`表示方式——`[]`；
- object：就是 JavaScript 的`{ ... }`表示方式。

以及上面的任意组合。

并且，JSON 还定死了字符集必须是 UTF-8，表示多语言就没有问题了。为了统一解析，JSON 的字符串规定必须用双引号`""`，Object 的键也必须用双引号`""`。而在 JavaScript 中，我们可以直接使用 JSON，因为 JavaScript 内置了 JSON 的解析。要使用 JSON，只需要对其序列化或反序列化即可。

##### 序列化

要序列化某个 JavaScript 对象，可以使用 `JSON.stringify` 方法，比如：

```javascript
var xiaoming = {
    name: '小明',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp']
};
var s = JSON.stringify(xiaoming);
```

`JSON.stringify(value[, replacer[, space[)` 定义如下：

- value: 必需， 要转换的 JavaScript 值（通常为对象或数组）。

- replacer: 可选。用于转换结果的函数或数组。

  如果 replacer 为**函数**，则 JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。

  如果 replacer 是一个**数组**，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。

- space: 可选，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 也可以使用非数字，如：\t。

另外，如果期望精确控制对象的序列化，则可以给对象定义一个 `toJson` 方法，并在其内直接返回序列化的结果，比如：

```javascript
var xiaoming = {
    name: '小明',
    age: 14,
    gender: true,
    height: 1.65,
    grade: null,
    'middle-school': '\"W3C\" Middle School',
    skills: ['JavaScript', 'Java', 'Python', 'Lisp'],
    toJSON: function () {
        return { // 只输出name和age，并且改变了key：
            'Name': this.name,
            'Age': this.age
        };
    }
};

JSON.stringify(xiaoming); // '{"Name":"小明","Age":14}'
```

##### 反序列化

拿到一个JSON格式的字符串，我们直接用`JSON.parse()`把它变成一个JavaScript对象：

```javascript
JSON.parse('[1,2,3,true]'); // [1, 2, 3, true]
JSON.parse('{"name":"小明","age":14}'); // Object {name: '小明', age: 14}
JSON.parse('true'); // true
JSON.parse('123.45'); // 123.45
```

`Json.parse(text[, reviver[)` 参数说明如下：

- **text:**必需， 一个有效的 JSON 字符串。
- **reviver:** 可选，一个转换结果的函数， 将为对象的每个成员调用此函数。

### 错误处理

JavaScript 通过 `try ... catch ... finally` 来进行错误处理，比如：

```javascript
var r1, r2, s = null;
try {
    r1 = s.length; // 此处应产生错误
    r2 = 100; // 该语句不会执行
} catch (e) {
    console.log('出错了：' + e); // TypeError: Cannot read property 'length' of null”。
} finally {
    console.log('finally');
}
```

##### 错误类型

JavaScript 有一个标准的`Error`对象表示错误，还有从`Error`派生的`TypeError`、`ReferenceError`等错误对象。我们在处理错误时，可以通过`catch(e)`捕获的变量`e`访问错误对象：

```javascript
try {
    ...
} catch (e) {
    if (e instanceof TypeError) {
        alert('Type error!');
    } else if (e instanceof Error) {
        alert(e.message);
    } else {
        alert('Error: ' + e);
    }
}
```

使用变量`e`是一个习惯用法，也可以以其他变量名命名，如`catch(ex)`。

##### 抛出错误

程序可以通过 `throw` 关键字主动抛出错误，比如：

```javascript
var r, n, s;
try {
    s = prompt('请输入一个数字');
    n = parseInt(s);
    if (isNaN(n)) {
        throw new Error('输入错误');
    }
    // 计算平方:
    r = n * n;
    console.log(n + ' * ' + n + ' = ' + r);
} catch (e) {
    console.log('出错了：' + e);
}
```

##### 错误传播

如果在一个函数内部发生了错误，它自身没有捕获，错误就会被抛到外层调用函数，如果外层函数也没有捕获，该错误会一直沿着函数调用链向上抛出，直到被 JavaScript 引擎捕获，代码终止执行。

##### 异步错误处理

编写 JavaScript 代码时，我们要时刻牢记，JavaScript 引擎是一个**事件驱动**的执行引擎，代码总是以单线程执行，而回调函数的执行需要等到下一个满足条件的事件出现后，才会被执行。

例如，`setTimeout()`函数可以传入回调函数，并在指定若干毫秒后执行：

```javascript
function printTime() {
    console.log('It is time!');
}

setTimeout(printTime, 1000);
console.log('done');
```

上面的代码会先打印`done`，1 秒后才会打印`It is time!`。

如果`printTime()`函数内部发生了错误，我们试图用 try 包裹`setTimeout()`是无效的：

```javascript
function printTime() {
    throw new Error();
}

try {
    setTimeout(printTime, 1000);
    console.log('done');
} catch (e) {
    console.log('error'); // 不会执行
}

```

原因就在于调用`setTimeout()`函数时，传入的`printTime`函数并未立刻执行！紧接着，JavaScript 引擎会继续执行`console.log('done');`语句，而此时并没有错误发生。直到 1 秒钟后，执行`printTime`函数时才发生错误，但此时除了在`printTime`函数内部捕获错误外，外层代码并无法捕获。

所以，涉及到异步代码，无法在调用时捕获，原因就是在捕获的当时，回调函数并未执行。

类似的，当我们处理一个事件时，在绑定事件的代码处，无法捕获事件处理函数的错误。

### 面向对象编程

JavaScript 不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。举个例子：

```javascript
var father = {xxx};
var son = {xxx};
son.__proto__ = father; // 将 son 的原型指向 father，从而使得 son ‘继承’ 了 father 的所有属性，且改变它的属性不影响 father 的属性
```

JavaScript 的原型链和 Java 的 Class 区别就在，它没有 “Class” 的概念，所有对象都是实例，所谓继承关系不过是把一个对象的原型指向另一个对象而已。而你完全可以**再次将对象的原型指向另一个对象**，此时**它继承的属性也会随之改变**，比如：

```javascript
son.__proto__ = father;
var father2 = {yyy: zzz};
son.__proto__ = father2;
```

***请注意***，在编写 JavaScript 代码时，**不要直接用`obj.__proto__`去改变一个对象的原型**，并且，低版本的IE也无法使用`__proto__`。`Object.create()`方法可以传入一个原型对象，并创建一个基于该原型的新对象，但是新对象什么属性都没有，因此，我们可以编写一个函数来创建`xiaoming`：

```javascript
// 原型对象:
var Student = {
    name: 'Robot',
    height: 1.2,
    run: function () {
        console.log(this.name + ' is running...');
    }
};

function createStudent(name) {
    // 基于Student原型创建一个新对象:
    var s = Object.create(Student);
    // 初始化新对象:
    s.name = name;
    return s;
}

var xiaoming = createStudent('小明');
xiaoming.run(); // 小明 is running...
xiaoming.__proto__ === Student; // true
```

这是创建原型继承的一种方法。

#### 创建对象

JavaScript 对每个创建的对象都会设置一个原型，指向它的原型对象，比如对于某个 Array 对象 arr，其原型链为：

```javascript
arr ----> Array.prototype ----> Object.prototype ----> null
```

同样对于某个函数 foo，由于它也是对象，所以其原型链为：

```javascript
foo ----> Function.prototype ----> Object.prototype ----> null
```

很容易想到，如果原型链很长，那么访问一个对象的属性就会因为花更多的时间查找而变得更慢，因此要**注意不要把原型链搞得太长**。

##### 构造函数

除了直接用`{ ... }`创建一个对象外，JavaScript 还可以用一种构造函数的方法来创建对象。它的用法是，先定义一个构造函数：

```javascript
function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
```

这一看只是一个普通的函数，但如果你用 `new` 关键字去调用它，它返回的就是一个对象，比如：

```javascript
var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明!
```

新创建的`xiaoming`的原型链是：

```javascript
xiaoming ----> Student.prototype ----> Object.prototype ----> null
```

也就是说，`xiaoming`的原型指向函数`Student`的原型。如果你又创建了`xiaohong`、`xiaojun`，那么这些对象的原型与`xiaoming`是一样的：

```javascript
xiaoming ↘
xiaohong -→ Student.prototype ----> Object.prototype ----> null
xiaojun  ↗
```

用`new Student()`创建的对象还从原型上获得了一个`constructor`属性，它指向函数`Student`本身：

```javascript
xiaoming.constructor === Student.prototype.constructor; // true
Student.prototype.constructor === Student; // true

Object.getPrototypeOf(xiaoming) === Student.prototype; // true

xiaoming instanceof Student; // true
```

看晕了吧？用一张图来表示这些乱七八糟的关系就是：

![protos](JavaScript%E5%85%A5%E9%97%A8.assets/l.png)

红色箭头是原型链。注意，`Student.prototype`指向的对象就是`xiaoming`、`xiaohong`的原型对象，这个原型对象自己还有个属性`constructor`，指向`Student`函数本身。

另外，函数`Student`恰好有个属性`prototype`指向`xiaoming`、`xiaohong`的原型对象，但是`xiaoming`、`xiaohong`这些对象可没有`prototype`这个属性，不过可以用`__proto__`这个非标准用法来查看。

现在我们就认为`xiaoming`、`xiaohong`这些对象“继承”自`Student`。

不过还有一个小问题，注意观察：

```javascript
xiaoming.name; // '小明'
xiaohong.name; // '小红'
xiaoming.hello; // function: Student.hello()
xiaohong.hello; // function: Student.hello()
xiaoming.hello === xiaohong.hello; // false
```

`xiaoming`和`xiaohong`各自的`name`不同，这是对的，否则我们无法区分谁是谁了。

`xiaoming`和`xiaohong`各自的`hello`是一个函数，但它们是两个不同的函数，虽然函数名称和代码都是相同的！

如果我们通过`new Student()`创建了很多对象，这些对象的`hello`函数实际上只需要共享同一个函数就可以了，这样可以节省很多内存。

要让创建的对象共享一个`hello`函数，根据对象的属性查找原则，我们只要把`hello`函数移动到`xiaoming`、`xiaohong`这些对象共同的原型上就可以了，也就是`Student.prototype`：

![protos2](https://s2.loli.net/2022/03/29/Cc348Zn1LY5DxIp.png)

修改代码如下：

```javascript
function Student(name) {
    this.name = name;
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};
```

用`new`创建基于原型的JavaScript的对象就是这么简单！

##### 忘记写 new  怎么办

如果一个函数被定义为用于创建对象的构造函数，但是调用时忘记了写`new`怎么办？

在strict模式下，`this.name = name`将报错，因为`this`绑定为`undefined`，在非strict模式下，`this.name = name`不报错，因为`this`绑定为`window`，于是无意间创建了全局变量`name`，并且返回`undefined`，这个结果更糟糕。

所以，调用构造函数千万不要忘记写`new`。为了区分普通函数和构造函数，按照约定，构造函数首字母应当大写，而普通函数首字母应当小写，这样，一些语法检查工具如[jslint](http://www.jslint.com/)将可以帮你检测到漏写的`new`。

最后，我们还可以编写一个`createStudent()`函数，在内部封装所有的`new`操作。一个常用的编程模式像这样：

```javascript
function Student(props) {
    this.name = props.name || '匿名'; // 默认值为'匿名'
    this.grade = props.grade || 1; // 默认值为1
}

Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
};

function createStudent(props) {
    return new Student(props || {})
}
```

这个`createStudent()`函数有几个巨大的优点：一是不需要`new`来调用，二是参数非常灵活，可以不传，也可以这么传：

```javascript
var xiaoming = createStudent({
    name: '小明'
});

xiaoming.grade; // 1
```

如果创建的对象有很多属性，我们只需要传递需要的某些属性，剩下的属性可以用默认值。由于参数是一个Object，我们无需记忆参数的顺序。如果恰好从`JSON`拿到了一个对象，就可以直接创建出`xiaoming`。

#### 原型继承

[参考页面](https://www.liaoxuefeng.com/wiki/1022910821149312/1023021997355072) （这一节感觉确实不太好写）

JavaScript 的原型继承实现方式就是：

1. 定义新的构造函数，并在内部用`call()`调用希望“继承”的构造函数，并绑定`this`；
2. 借助中间函数`F`实现原型链继承，最好通过封装的`inherits`函数完成；
3. 继续在新的构造函数的原型上定义新方法。

#### Class 继承

上一节提到的原型继承实现起来过于麻烦，在 ES6 中新引入了关键字 `class` 用于编写类，比如：

```javascript
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```

最后，创建一个`Student`对象代码和前面章节完全一样：

```javascript
var xiaoming = new Student('小明');
xiaoming.hello();
```

##### class 继承

有了 `class` 后，想要继承某个类只需要使用 `extends` 关键字，比如：

```javascript
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }

    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```

注意子类和父类的构造器参数可以不一样，但是对于继承自父类的属性，一定要通过 `super()` 方法去初始化。

ES6 引入的`class`和原有的 JavaScript 原型继承没有任何区别，使用起来方便，但不是所有主流浏览器都支持 ES6 的 class，所以如果一定要现在就用上，就需要一个工具把`class`代码转换为传统的`prototype`代码，可以试试 [Babel](https://babeljs.io/) 这个工具。

## 浏览器

不同的浏览器对 JavaScript 支持的差异主要是，有些 API 的接口不一样，比如 AJAX，File 接口。对于 ES6 标准，不同的浏览器对各个特性支持也不一样。

在编写 JavaScript 的时候，就要充分考虑到浏览器的差异，尽量让同一份 JavaScript 代码能运行在不同的浏览器中。

另外还要注意识别各种国产浏览器，如某某安全浏览器，某某旋风浏览器，它们只是做了一个壳，其核心调用的是 IE，也有号称同时支持 IE 和 Webkit 的“双核”浏览器。

### 浏览器对象

JavaScript 可以获取浏览器提供的很多对象，并进行操作。

##### window

`window` 对象不但充当全局作用域，而且表示浏览器窗口。

`window` 对象有 `innerWidth`和`innerHeight`属性，可以获取浏览器窗口的内部宽度和高度。内部宽高是指除去菜单栏、工具栏、边框等占位元素后，用于显示网页的净宽高。

兼容性：IE<=8 不支持。

对应的，还有一个`outerWidth`和`outerHeight`属性，可以获取浏览器窗口的整个宽高。

##### navigator

`navigator` 对象表示浏览器的信息，最常用的属性包括：

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的`User-Agent`字符串。

*请注意*，**`navigator`的信息可以很容易地被用户修改**，所以 JavaScript 读取的值**不一定是正确**的。

正确的方法是充分利用 JavaScript 对不存在属性返回`undefined`的特性，直接用短路运算符`||`计算来针对不同浏览器编写不同的代码，比如获取页面宽度可以这么写：

```javascript
var width = window.innerWidth || document.body.clientWidth;
```

##### screen

`screen`对象表示屏幕的信息，常用的属性有：

- screen.width：屏幕宽度，以像素为单位；
- screen.height：屏幕高度，以像素为单位；
- screen.colorDepth：返回颜色位数，如 8、16、24。

##### location

`location`对象表示当前页面的 URL 信息。为了获取 URL 的信息，可以这么写：

```javascript
// URL: http://www.example.com:8080/path/index.html?a=1&b=2#TOP
location.href; // 'http://www.example.com:8080/path/index.html?a=1&b=2#TOP'
location.protocol; // 'http'
location.host; // 'www.example.com'
location.port; // '8080'
location.pathname; // '/path/index.html'
location.search; // '?a=1&b=2'
location.hash; // 'TOP'
```

要加载一个新页面，可以调用`location.assign()`。如果要重新加载当前页面，调用`location.reload()`方法非常方便，比如：

```javascript
location.assign("https://www.baidu.com"); // 转到百度
location.reload();
```

##### document

`document`对象表示当前页面。由于 HTML 在浏览器中以 DOM 形式表示为树形结构，`document`对象就是整个DOM 树的根节点。

`document` 的 `title`属性是从 HTML 文档中的 `<title>xxx</title>`读取的，但是可以动态改变：

```javascript
document.title = 'xxx';
```

要查找 DOM 树的某个节点，需要从`document`对象开始查找。最常用的查找是根据 ID 和 Tag Name，也可以根据 Name，Class Name 来找，比如：

```javascript
document.getElementById(id); // 不存在则返回 null
document.getElementsByTagName(tagName); // 返回一个HTMLCollection，不存在为空，可迭代，下同
document.getElementsByName(name);
document.getElementsByClassName(className);
```

`document`对象还有一个`cookie`属性，可以获取当前页面的 Cookie，即 `document.cookie`。

服务器在设置 Cookie 时可以使用`httpOnly`，**设定了`httpOnly`的 Cookie 将不能被 JavaScript 读取**。这个行为由浏览器实现，主流浏览器均支持`httpOnly`选项，IE 从 IE6 SP1 开始支持。

为了确保安全，服务器端在设置 Cookie 时，应该始终坚持使用`httpOnly`。

##### history

`history`对象保存了浏览器的历史记录，JavaScript 可以调用`history`对象的`back()`或`forward ()`，相当于用户点击了浏览器的“后退”或“前进”按钮。

这个对象属于历史遗留对象，对于现代 Web 页面来说，由于大量使用 AJAX 和页面交互，简单粗暴地调用`history.back()`可能会让用户感到非常愤怒。

新手开始设计 Web 页面时喜欢在登录页登录成功时调用`history.back()`，试图回到登录前的页面。这是一种错误的方法。

**任何情况，你都不应该使用`history`这个对象了**。

### 操作 DOM

始终记住 DOM 是一个树形结构。操作一个 DOM 节点实际上就是这么几个操作：

- 更新：更新该 DOM 节点的内容，相当于更新了该 DOM 节点表示的 HTML 的内容；
- 遍历：遍历该 DOM 节点下的子节点，以便进行进一步操作；
- 添加：在该 DOM 节点下新增一个子节点，相当于动态增加了一个 HTML 节点；
- 删除：将该节点从 HTML 中删除，相当于删掉了该 DOM 节点的内容以及它包含的所有子节点。

要操作 DOM，第一种方法是通过 `getElementByXXX` 来实现。而要精确地选择 DOM，可以先定位父节点，再从父节点开始选择，以缩小范围，比如：

```javascript
// 返回ID为'test'的节点：
var test = document.getElementById('test');

// 先定位ID为'test-table'的节点，再返回其内部所有tr节点：
var trs = document.getElementById('test-table').getElementsByTagName('tr');

// 先定位ID为'test-div'的节点，再返回其内部所有class包含red的节点：
var reds = document.getElementById('test-div').getElementsByClassName('red');

// 获取节点test下的所有直属子节点:
var cs = test.children;

// 获取节点test下第一个、最后一个子节点：
var first = test.firstElementChild;
var last = test.lastElementChild;
```

第二种方法是使用 `querySelector()` 和 `querySelectorAll()`，需要了解 selector 语法，然后使用条件来获取节点，更加方便：

```javascript
// 通过querySelector获取ID为q1的节点：
var q1 = document.querySelector('#q1');

// 通过querySelectorAll获取q1节点内的符合条件的所有节点：
var ps = q1.querySelectorAll('div.highlighted > p');
```

注意：低版本的 IE<8 不支持`querySelector`和`querySelectorAll`。IE8 仅有限支持。

严格地讲，我们这里的 DOM 节点是指`Element`，但是 DOM 节点实际上是`Node`，在 HTML 中，`Node`包括`Element`、`Comment`、`CDATA_SECTION`等很多种，以及根节点`Document`类型，但是，绝大多数时候我们只关心`Element`，也就是实际控制页面结构的`Node`，其他类型的`Node`忽略即可。根节点`Document`已经自动绑定为全局变量`document`。

#### 更新 DOM

拿到一个 DOM 节点后，我们可以对它进行更新。

可以直接修改节点的文本，方法有两种：

一种是修改`innerHTML`属性，这个方式非常强大，不但可以修改一个 DOM 节点的文本内容，还可以直接通过  HTML 片段修改 DOM 节点内部的子树：

```javascript
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本为abc:
p.innerHTML = 'ABC'; // <p id="p-id">ABC</p>
// 设置HTML:
p.innerHTML = 'ABC <span style="color:red">RED</span> XYZ';
// <p>...</p>的内部结构已修改
```

用`innerHTML`时要注意，是否需要写入 HTML。如果写入的字符串是通过网络拿到的，要注意对字符编码来避免 XSS 攻击。

第二种是修改`innerText`或`textContent`属性，这样可以自动对字符串进行 HTML 编码，保证无法设置任何HTML 标签：

```javascript
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置文本:
p.innerText = '<script>alert("Hi")</script>';
// HTML被自动编码，无法设置一个<script>节点:
// <p id="p-id">&lt;script&gt;alert("Hi")&lt;/script&gt;</p>
```

两者的区别在于读取属性时，`innerText`不返回隐藏元素的文本，而`textContent`返回所有文本。另外注意IE<9 不支持`textContent`。

修改 CSS 也是经常需要的操作。DOM 节点的`style`属性对应所有的 CSS，可以直接获取或设置。**因为 CSS 允许`font-size`这样的名称，但它并非 JavaScript 有效的属性名，所以需要在 JavaScript 中改写为驼峰式命名`fontSize`**：

```javascript
// 获取<p id="p-id">...</p>
var p = document.getElementById('p-id');
// 设置CSS:
p.style.color = '#ff0000';
p.style.fontSize = '20px';
p.style.paddingTop = '2em';
```

#### 插入 DOM

当我们获得了某个 DOM 节点，想在这个 DOM 节点内插入新的 DOM，应该如何做？

如果这个 DOM 节点是空的，例如，`<div></div>`，那么，直接使用`innerHTML = '<span>child</span>'`就可以修改 DOM 节点的内容，相当于“插入”了新的 DOM 节点。

如果这个 DOM 节点不是空的，那就不能这么做，因为`innerHTML`会直接替换掉原来的所有子节点。

有两个办法可以插入新的节点。一个是使用`appendChild`，把一个子节点添加到父节点的最后一个子节点。例如：

```html
<!-- HTML结构 -->
<p id="js">JavaScript</p>
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
</div>
```

把`<p id="js">JavaScript</p>`添加到`<div id="list">`的最后一项：

```javascript
var
    js = document.getElementById('js'),
    list = document.getElementById('list');
list.appendChild(js);
```

现在，HTML 结构变成了这样：

```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
    <p id="js">JavaScript</p>
</div>
```

**因为我们插入的`js`节点已经存在于当前的文档树，因此这个节点首先会从原先的位置删除，再插入到新的位置**。

更多的时候我们会从零创建一个新的节点，然后插入到指定位置：

```javascript
var
    list = document.getElementById('list'),
    haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.appendChild(haskell);
```

这样我们就动态添加了一个新的节点：

```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
    <p id="haskell">Haskell</p>
</div>
```

##### insertBefore

如果我们要把子节点插入到指定的位置怎么办？可以使用`parentElement.insertBefore(newElement, referenceElement);`，子节点会插入到`referenceElement`之前。

还是以上面的 HTML 为例，假定我们要把`Haskell`插入到`Python`之前：

```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
</div>
```

可以这么写：

```javascript
var
    list = document.getElementById('list'),
    ref = document.getElementById('python'),
    haskell = document.createElement('p');
haskell.id = 'haskell';
haskell.innerText = 'Haskell';
list.insertBefore(haskell, ref);
```

新的HTML结构如下：

```html
<!-- HTML结构 -->
<div id="list">
    <p id="java">Java</p>
    <p id="haskell">Haskell</p>
    <p id="python">Python</p>
    <p id="scheme">Scheme</p>
</div>
```

可见，使用`insertBefore`重点是要拿到一个“参考子节点”的引用。很多时候，需要循环一个父节点的所有子节点，可以通过迭代`children`属性实现：

```javascript
var
    i, c,
    list = document.getElementById('list');
for (i = 0; i < list.children.length; i++) {
    c = list.children[i]; // 拿到第i个子节点
}
```

#### 删除 DOM

要删除一个节点，首先要获得该节点本身以及它的父节点，然后，调用父节点的`removeChild`把自己删掉：

```javascript
// 拿到待删除节点:
var self = document.getElementById('to-be-removed');
// 拿到父节点:
var parent = self.parentElement;
// 删除:
var removed = parent.removeChild(self);
removed === self; // true
```

**注意到删除后的节点虽然不在文档树中了，但其实它还在内存中，可以随时再次被添加到别的位置**。

当你遍历一个父节点的子节点并进行删除操作时，要注意，`children`属性是一个只读属性，并且它在子节点变化时会实时更新。

例如，对于如下HTML结构：

```html
<div id="parent">
    <p>First</p>
    <p>Second</p>
</div>
```

当我们用如下代码删除子节点时：

```javascript
var parent = document.getElementById('parent');
parent.removeChild(parent.children[0]);
parent.removeChild(parent.children[1]); // <-- 浏览器报错
```

浏览器报错：`parent.children[1]`不是一个有效的节点。原因就在于，当`<p>First</p>`节点被删除后，`parent.children`的节点数量已经从2变为了1，索引`[1]`已经不存在了。

因此，**删除多个节点时，要注意`children`属性时刻都在变化**。

### 操作表单

用 JavaScript 操作表单（`input` 和 `select` 标签）和操作 DOM 是类似的，因为表单本身也是 DOM 树。

#### 获取值

如果我们获得了一个`<input>`节点的引用，就可以直接调用`value`获得对应的用户输入值：

```javascript
// <input type="text" id="email">
var input = document.getElementById('email');
input.value; // '用户输入的值'
```

这种方式可以应用于`text`、`password`、`hidden`以及`select`。但是，对于单选框和复选框，`value`属性返回的永远是HTML预设的值，而我们需要获得的实际是用户是否“勾上了”选项，所以应该用`checked`判断：

```javascript
// <label><input type="radio" name="weekday" id="monday" value="1"> Monday</label>
// <label><input type="radio" name="weekday" id="tuesday" value="2"> Tuesday</label>
var mon = document.getElementById('monday');
var tue = document.getElementById('tuesday');
mon.value; // '1'
tue.value; // '2'
mon.checked; // true或者false
tue.checked; // true或者false
```

#### 设置值

设置值和获取值类似，对于`text`、`password`、`hidden`以及`select`，直接设置`value`就可以：

```javascript
// <input type="text" id="email">
var input = document.getElementById('email');
input.value = 'test@example.com'; // 文本框的内容已更新
```

对于单选框和复选框，设置`checked`为`true`或`false`即可。

#### HTML5 控件

HTML5 新增了大量标准控件，常用的包括`date`、`datetime`、`datetime-local`、`color`等，它们都使用`<input>`标签。不支持 HTML5 的浏览器无法识别新的控件，会把它们当做`type="text"`来显示。支持 HTML5 的浏览器将获得格式化的字符串。例如，`type="date"`类型的`input`的`value`将保证是一个有效的`YYYY-MM-DD`格式的日期，或者空字符串。

#### 提交表单

JavaScript 可以以两种方式来处理表单的提交（AJAX 方式在后面章节介绍）。

方式一是通过`<form>`元素的`submit()`方法提交一个表单，例如，响应一个`<button>`的`click`事件，在JavaScript 代码中提交表单：

```html
<!-- HTML -->
<form id="test-form">
    <input type="text" name="test">
    <button type="button" onclick="doSubmitForm()">Submit</button>
</form>

<script>
function doSubmitForm() {
    var form = document.getElementById('test-form');
    // 可以在此修改form的input...
    // 提交form:
    form.submit();
}
</script>
```

这种方式的缺点是扰乱了浏览器对 form 的正常提交。浏览器默认点击`<button type="submit">`时提交表单，或者用户在最后一个输入框按回车键。

因此，第二种方式是响应`<form>`本身的`onsubmit`事件，在提交 form 时作修改：

```html
<!-- HTML -->
<form id="test-form" onsubmit="return checkForm()">
    <input type="text" name="test">
    <button type="submit">Submit</button>
</form>

<script>
function checkForm() {
    var form = document.getElementById('test-form');
    // 可以在此修改form的input...
    // 继续下一步:
    return true;
}
</script>
```

注意要`return true`来告诉浏览器继续提交，如果`return false`，浏览器将不会继续提交 form，这种情况通常对应用户输入有误，提示用户错误信息后终止提交 form。

在检查和修改`<input>`时，要充分利用`<input type="hidden">`来传递数据。

### 操作文件

在 HTML 表单中，可以上传文件的唯一控件就是`<input type="file">`。

*注意*：当一个表单包含`<input type="file">`时，表单的`enctype`必须指定为`multipart/form-data`，`method`必须指定为`post`，浏览器才能正确编码并以`multipart/form-data`格式发送表单的数据。

出于安全考虑，浏览器只允许用户点击`<input type="file">`来选择本地文件，用 JavaScript 对`<input type="file">`的`value`赋值是没有任何效果的。当用户选择了上传某个文件后，JavaScript 也无法获得该文件的真实路径。

通常，上传的文件都由后台服务器处理，JavaScript 可以在提交表单时对文件扩展名做检查，以便防止用户上传无效格式的文件

#### File API

由于 JavaScript 对用户上传的文件操作非常有限，尤其是无法读取文件内容，使得很多需要操作文件的网页不得不用 Flash 这样的第三方插件来实现。

随着 HTML5 的普及，新增的 File API 允许 JavaScript 读取文件内容，获得更多的文件信息。

HTML5 的 File API 提供了`File`和`FileReader`两个主要对象，可以获得文件信息并读取文件。

```javascript
var
    fileInput = document.getElementById('test-image-file'),
    info = document.getElementById('test-file-info'),
    preview = document.getElementById('test-image-preview');
// 监听change事件:
fileInput.addEventListener('change', function () {
    // 清除背景图片:
    preview.style.backgroundImage = '';
    // 检查文件是否选择:
    if (!fileInput.value) {
        info.innerHTML = '没有选择文件';
        return;
    }
    // 获取File引用:
    var file = fileInput.files[0];
    // 获取File信息:
    info.innerHTML = '文件: ' + file.name + '<br>' +
                     '大小: ' + file.size + '<br>' +
                     '修改: ' + file.lastModified;
    if (file.type !== 'image/jpeg' && file.type !== 'image/png' && file.type !== 'image/gif') {
        alert('不是有效的图片文件!');
        return;
    }
    // 读取文件:
    var reader = new FileReader();
    reader.onload = function(e) {
        var
            data = e.target.result; // 'data:image/jpeg;base64,/9j/4AAQSk...(base64编码)...'            
        preview.style.backgroundImage = 'url(' + data + ')';
    };
    // 以DataURL的形式读取文件:
    reader.readAsDataURL(file);
});
```

上面的代码演示了如何通过 HTML5 的 File API 读取文件内容。以 DataURL 的形式读取到的文件是一个字符串，类似于`data:image/jpeg;base64,/9j/4AAQSk...(base64编码)...`，常用于设置图像。如果需要服务器端处理，把字符串`base64,`后面的字符发送给服务器并用 Base64 解码就可以得到原始文件的二进制内容。

#### 回调

上面的代码还演示了 JavaScript 的一个重要的特性就是单线程执行模式。在 JavaScript 中，浏览器的 JavaScript执行引擎在执行 JavaScript 代码时，总是以单线程模式执行，也就是说，任何时候，JavaScript 代码都不可能同时有多于 1 个线程在执行。

在JavaScript中，执行多任务实际上都是异步调用，比如上面的代码：

```javascript
reader.readAsDataURL(file);
```

就会发起一个异步操作来读取文件内容。因为是异步操作，所以我们在 JavaScript 代码中就不知道什么时候操作结束，因此需要先设置一个回调函数：

```javascript
reader.onload = function(e) {
    // 当文件读取完成后，自动调用此函数:
};
```

当文件读取完成后，JavaScript 引擎将自动调用我们设置的回调函数。执行回调函数时，文件已经读取完毕，所以我们可以在回调函数内部安全地获得文件内容。

### AJAX

AJAX 不是 JavaScript 的规范，它只是一个哥们“发明”的缩写：Asynchronous JavaScript and XML，意思就是用JavaScript 执行异步网络请求。

用 JavaScript 写一个完整的 AJAX 代码并不复杂，但是需要注意：AJAX 请求是异步执行的，也就是说，要通过回调函数获得响应。

在现代浏览器上写 AJAX 主要依靠`XMLHttpRequest`对象：

```javascript
function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}

var request = new XMLHttpRequest(); // 新建XMLHttpRequest对象

request.onreadystatechange = function () { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
        // 判断响应结果:
        if (request.status === 200) {
            // 成功，通过responseText拿到响应的文本:
            return success(request.responseText);
        } else {
            // 失败，根据响应码判断失败原因:
            return fail(request.status);
        }
    } else {
        // HTTP请求还在继续...
    }
}

// 发送请求:
request.open('GET', '/api/categories');
request.send();

alert('请求已发送，请等待响应...');
```

对于低版本的 IE，需要换一个`ActiveXObject`对象：

```javascript
function success(text) {
    var textarea = document.getElementById('test-ie-response-text');
    textarea.value = text;
}

function fail(code) {
    var textarea = document.getElementById('test-ie-response-text');
    textarea.value = 'Error code: ' + code;
}

var request = new ActiveXObject('Microsoft.XMLHTTP'); // 新建Microsoft.XMLHTTP对象

request.onreadystatechange = function () { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
        // 判断响应结果:
        if (request.status === 200) {
            // 成功，通过responseText拿到响应的文本:
            return success(request.responseText);
        } else {
            // 失败，根据响应码判断失败原因:
            return fail(request.status);
        }
    } else {
        // HTTP请求还在继续...
    }
}

// 发送请求:
request.open('GET', '/api/categories');
request.send();

alert('请求已发送，请等待响应...');
```

如果你想把标准写法和IE写法混在一起，可以这么写：

```javascript
var request;
if (window.XMLHttpRequest) {
    request = new XMLHttpRequest();
} else {
    request = new ActiveXObject('Microsoft.XMLHTTP');
}
```

通过检测`window`对象是否有`XMLHttpRequest`属性来确定浏览器是否支持标准的`XMLHttpRequest`。注意，*不要*根据浏览器的`navigator.userAgent`来检测浏览器是否支持某个 JavaScript 特性，一是因为这个字符串本身可以伪造，二是通过 IE 版本判断 JavaScript 特性将非常复杂。

当创建了`XMLHttpRequest`对象后，要先设置`onreadystatechange`的回调函数。在回调函数中，通常我们只需通过`readyState === 4`判断请求是否完成，如果已完成，再根据`status === 200`判断是否是一个成功的响应。

`XMLHttpRequest`对象的`open()`方法有 3 个参数，第一个参数指定是`GET`还是`POST`，第二个参数指定URL地址，第三个参数指定是否使用异步，默认是`true`，所以不用写。

*注意*，千万不要把第三个参数指定为`false`，否则浏览器将停止响应，直到 AJAX 请求完成。如果这个请求耗时10 秒，那么 10 秒内你会发现浏览器处于“假死”状态。

最后调用`send()`方法才真正发送请求。`GET`请求不需要参数，`POST`请求需要把body部分以字符串或者`FormData`对象传进去。

#### 安全限制

上面代码的 URL 使用的是相对路径。如果你把它改为`'http://www.sina.com.cn/'`，再运行，肯定报错。这是因为浏览器的同源策略导致的。默认情况下，JavaScript 在发送 AJAX 请求时，URL 的域名必须和当前页面完全一致（包括**一致的域名、协议和端口**）。

如果要用 JavaScript 请求外域，方法大概有这么几种：

一是通过 Flash 插件发送 HTTP 请求，这种方式可以绕过浏览器的安全限制，但必须安装 Flash，并且跟 Flash 交互。不过 Flash 用起来麻烦，而且现在用得也越来越少了。

二是通过在同源域名下架设一个代理服务器来转发，JavaScript 负责把请求发送到代理服务器：

```html
'/proxy?url=http://www.sina.com.cn'
```

代理服务器再把结果返回，这样就遵守了浏览器的同源策略。这种方式麻烦之处在于需要服务器端额外做开发。

第三种方式称为 JSONP，它有个限制，**只能用 GET 请求**，并且要求返回 JavaScript。这种方式跨域实际上是利用了**浏览器允许跨域引用 JavaScript 资源**：

```html
<html>
<head>
    <script src="http://example.com/abc.js"></script>
    ...
</head>
<body>
...
</body>
</html>
```

JSONP通常以函数调用的形式返回，例如，返回JavaScript内容如下：

```javascript
foo('data');
```

这样一来，我们如果在页面中先准备好`foo()`函数，然后给页面动态加一个`<script>`节点，相当于动态读取外域的 JavaScript 资源，最后就等着接收回调了。

#### CORS

如果浏览器支持 HTML5，那么就可以一劳永逸地使用新的跨域策略：CORS了。

CORS 全称 Cross-Origin Resource Sharing，是 HTML5 规范定义的如何跨域访问资源。

下面是一个示例图：

![js-cors](JavaScript%E5%85%A5%E9%97%A8.assets/l.png)

跨域能否成功，取决于对方服务器是否愿意给你设置一个正确的`Access-Control-Allow-Origin`，决定权始终在对方手中。

最新的浏览器全面支持 HTML5。在引用外域资源时，除了 JavaScript 和 CSS 外，都要验证 CORS。

上面这种跨域请求，称之为“简单请求”。简单请求包括 GET、HEAD 和 POST（POST 的 Content-Type 类型 仅限`application/x-www-form-urlencoded`、`multipart/form-data`和`text/plain`），并且不能出现任何自定义头（例如，`X-Custom: 12345`），通常能满足 90% 的需求。

对于 PUT、DELETE 以及其他类型如`application/json`的 POST 请求，在发送 AJAX 请求之前，浏览器会先发送一个`OPTIONS`请求（称为preflighted请求）到这个 URL 上，询问目标服务器是否接受：

```html
OPTIONS /path/to/resource HTTP/1.1
Host: bar.com
Origin: http://my.com
Access-Control-Request-Method: POST
```

服务器必须响应并明确指出允许的Method：

```html
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://my.com
Access-Control-Allow-Methods: POST, GET, PUT, OPTIONS
Access-Control-Max-Age: 86400
```

浏览器确认服务器响应的`Access-Control-Allow-Methods`头确实包含将要发送的 AJAX 请求的 Method，才会继续发送 AJAX，否则，抛出一个错误。

由于以`POST`、`PUT`方式传送 JSON 格式的数据在 REST 中很常见，所以要跨域正确处理`POST`和`PUT`请求，服务器端必须正确响应`OPTIONS`请求。

需要深入了解CORS的童鞋请移步 [W3C文档](http://www.w3.org/TR/cors/)。

### Promise

上面提到 JavaScript 中所有代码都是单线程执行，因此所有网络操作、浏览器事件都必须是异步执行。尽管异步执行可以通过回调函数实现，但把回调函数写入到 AJAX 操作中不好看且不利于代码复用。那么是否能写成类似下面的写法：

```javascript
var ajax = ajaxGet('http://...');
ajax.ifSuccess(success)
    .ifFail(fail);
```

这种写法好处在于先统一执行 AJAX 逻辑，不关心如何处理结果，然后，根据结果是成功还是失败，在将来的某个时候调用`success`函数或`fail`函数。

而这种“承诺将来会执行”的对象在 JavaScript 中称为 Promise 对象。

Promise 有各种开源实现，在 ES6 中被统一规范，由浏览器直接支持。

我们先看一个最简单的 Promise 例子：生成一个 0-2 之间的随机数，如果小于 1，则等待一段时间后返回成功，否则返回失败：

```javascript
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function () {
        if (timeOut < 1) {
            log('call resolve()...');
            resolve('200 OK');
        }
        else {
            log('call reject()...');
            reject('timeout in ' + timeOut + ' seconds.');
        }
    }, timeOut * 1000);
}
```

这个`test()`函数有两个参数，这两个参数都是函数，如果执行成功，我们将调用`resolve('200 OK')`，如果执行失败，我们将调用`reject('timeout in ' + timeOut + ' seconds.')`。可以看出，`test()`函数只关心自身的逻辑，并不关心具体的`resolve`和`reject`将如何处理结果。

有了执行函数，我们就可以用一个 Promise 对象来执行它，并在将来某个时刻获得成功或失败的结果：

```javascript
var p1 = new Promise(test); // Promise 对象，负责执行 test 函数
// 如果成功，执行这个函数：
var p2 = p1.then(function (result) {
    console.log('成功：' + result);
});
// 如果失败，执行这个函数
var p3 = p2.catch(function (reason) {
    console.log('失败：' + reason);
});
```

Promise对象可以串联起来，所以上述代码可以简化为：

```javascript
new Promise(test).then(function (result) {
    console.log('成功：' + result);
}).catch(function (reason) {
    console.log('失败：' + reason);
});
```

**注意 Promise 无论成功或者失败都会进入 then，在 then 中接受错误的对象后再进入的 catch，所以书写 catch 和 then 时要注意二者的位置。**

Promise 最大的好处是在异步执行的流程中，把执行代码和处理结果的代码清晰地分离。

Promise 还可以做更多的事情，比如，有若干个异步任务，需要先做任务 1，如果成功后再做任务 2，任何任务失败则不再继续并执行错误处理函数。

要串行执行这样的异步任务，不用 Promise 需要写一层一层的嵌套代码。有了 Promise，我们只需要简单地写：

```javascript
job1.then(job2).then(job3).catch(handleError);
```

其中，`job1`、`job2`和`job3`都是 Promise 对象。

除了串行执行若干异步任务外，Promise 还可以并行执行异步任务。

试想一个页面聊天系统，我们需要从两个不同的 URL 分别获得用户的个人信息和好友列表，这两个任务是可以并行执行的，用`Promise.all()`实现如下：

```javascript
var p1 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 500, 'P1');
});
var p2 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 600, 'P2');
});
// 同时执行p1和p2，并在它们都完成后执行then:
Promise.all([p1, p2]).then(function (results) {
    console.log(results); // 获得一个Array: ['P1', 'P2']
});
```

有些时候，多个异步任务是为了容错。比如，同时向两个 URL 读取用户的个人信息，只需要获得先返回的结果即可。这种情况下，用`Promise.race()`实现：

```javascript
var p1 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 500, 'P1');
});
var p2 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 600, 'P2');
});
Promise.race([p1, p2]).then(function (result) {
    console.log(result); // 'P1'
});
```

由于`p1`执行较快，Promise的`then()`将获得结果`'P1'`。`p2`仍在继续执行，但执行结果将被丢弃。

如果我们组合使用 Promise，就可以把很多异步任务以并行和串行的方式组合起来执行。

### Canvas

Canvas 是 HTML5 新增的组件，它就像一块幕布，可以用 JavaScript 在上面绘制各种图表、动画等。

一个 Canvas 定义了一个指定尺寸的矩形框，在这个范围内我们可以随意绘制：

```html
<canvas id="test-canvas" width="300" height="200"></canvas>
```

由于浏览器对 HTML5 标准支持不一致，所以，通常在`<canvas>`内部添加一些说明性 HTML 代码，如果浏览器支持 Canvas，它将忽略`<canvas>`内部的 HTML，如果浏览器不支持 Canvas，它将显示`<canvas>`内部的HTML：

```
<canvas id="test-stock" width="300" height="200">
    <p>Current Price: 25.51</p>
</canvas>
```

在使用 Canvas 前，用`canvas.getContext`来测试浏览器是否支持 Canvas：

```html
<!-- HTML代码 -->
<canvas id="test-canvas" width="200" heigth="100">
    <p>你的浏览器不支持Canvas</p>
</canvas>
```

```javascript
var canvas = document.getElementById('test-canvas');
if (canvas.getContext) {
    console.log('你的浏览器支持Canvas!');
} else {
    console.log('你的浏览器不支持Canvas!');
}
```

`getContext('2d')`方法让我们拿到一个`CanvasRenderingContext2D`对象，所有的绘图操作都需要通过这个对象完成。

```javascript
var ctx = canvas.getContext('2d');
```

如果需要绘制 3D 怎么办？HTML5 还有一个 WebGL 规范，允许在 Canvas 中绘制 3D 图形：

```javascript
gl = canvas.getContext("webgl");
```

本节我们只专注于绘制 2D 图形。

#### 绘制形状

在绘制前，我们需要先了解一下 Canvas 的坐标系统。Canvas 的坐标以左上角为原点，水平向右为 X 轴，垂直向下为 Y 轴，**以像素为单位**，所以每个点都是非负整数。

`CanvasRenderingContext2D`对象有若干方法来绘制图形：

```javascript
ctx.clearRect(0, 0, 200, 200); // 擦除(0,0)位置大小为200x200的矩形，擦除的意思是把该区域变为透明
ctx.fillStyle = '#dddddd'; // 设置颜色
ctx.fillRect(10, 10, 130, 130); // 把(10,10)位置大小为130x130的矩形涂色
// 利用Path绘制复杂路径:
var path=new Path2D();
path.arc(75, 75, 50, 0, Math.PI*2, true);
path.moveTo(110,75);
ctx.strokeStyle = '#0000ff';
ctx.stroke(path);
```

#### 绘制文本

绘制文本就是在指定的位置输出文本，可以设置文本的字体、样式、阴影等，与 CSS 完全一致：

```javascript
ctx.clearRect(0, 0, canvas.width, canvas.height);
ctx.shadowOffsetX = 2;
ctx.shadowOffsetY = 2;
ctx.shadowBlur = 2;
ctx.shadowColor = '#666666';
ctx.font = '24px Arial';
ctx.fillStyle = '#333333';
ctx.fillText('带阴影的文字', 20, 40);
```

#### 复杂绘制建议

Canvas 除了能绘制基本的形状和文本，还可以实现动画、缩放、各种滤镜和像素转换等高级操作。如果要实现非常复杂的操作，考虑以下优化方案：

- 通过创建一个不可见的 Canvas 来绘图，然后将最终绘制结果复制到页面的可见 Canvas 中；
- 尽量使用整数坐标而不是浮点数；
- 可以创建多个重叠的 Canvas 绘制不同的层，而不是在一个 Canvas 中绘制非常复杂的图；
- 背景图片如果不变可以直接用`<img>`标签并放到最底层。

## jQuery

jQuery 是 JavaScript 世界中使用最广泛的库，它能帮我们干这些事情：

- 消除浏览器差异：你不需要自己写冗长的代码来针对不同的浏览器来绑定事件，编写 AJAX 等代码；
- 简洁的操作 DOM 的方法：写`$('#test')`肯定比`document.getElementById('test')`来得简洁；
- 轻松实现动画、修改 CSS 等各种操作。

jQuery 的理念 “Write Less, Do More“，让你写更少的代码，完成更多的工作！

目前 jQuery 有 1.x 和 2.x 两个主要版本，区别在于 2.x 移除了对古老的 IE 6、7、8的支持，因此 2.x 的代码更精简。选择哪个版本主要取决于你是否想支持 IE 6~8。

从 [jQuery官网](http://jquery.com/download/)可以下载最新版本。jQuery 只是一个`jquery-xxx.js`文件，但你会看到有 compressed（已压缩）和 uncompressed（未压缩）两种版本，使用时完全一样，但如果你想深入研究 jQuery 源码，那就用uncompressed 版本。

##### 引入 jQuery

要使用 jQuery，只需在页面的 `<head>` 引入 jQuery 文件即可：

```html
<html>
	<head>
		<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
		...
     </head>
</html>
```

##### $ 符号

`$`是著名的 jQuery 符号。实际上，jQuery 把所有功能全部封装在一个全局变量`jQuery`中，而`$`也是一个合法的变量名，它是变量`jQuery`的别名：

```javascript
window.jQuery; // jQuery(selector, context)
window.$; // jQuery(selector, context)
$ === jQuery; // true
typeof($); // 'function'
```

`$`本质上就是一个函数，但是函数也是对象，于是`$`除了可以直接调用外，也可以有很多其他属性。

*注意*，你看到的`$`函数名可能不是`jQuery(selector, context)`，因为很多 JavaScript 压缩工具可以对函数名和参数改名，所以压缩过的 jQuery 源码`$`函数可能变成`a(b, c)`。

绝大多数时候，我们都直接用`$`（因为写起来更简单嘛）。但是，如果`$`这个变量不幸地被占用了，而且还不能改，那我们就只能让`jQuery`把`$`变量交出来，然后就只能使用`jQuery`这个变量：

```javascript
$; // jQuery(selector, context)
jQuery.noConflict();
$; // undefined
jQuery; // jQuery(selector, context)
```

这种黑魔法的原理是 jQuery 在占用`$`之前，先在内部保存了原来的`$`,调用`jQuery.noConflict()`时会把原来保存的变量还原。

### 选择器

选择器是 jQuery 的核心，它用于帮助我们快速定位到一个或多个 DOM 节点。

##### 按 ID 查找

类似 CSS 中的选择，jQuery 中按 ID 查找可类似这样：

```javascript
var result = $('#id'); // jQuery 对象
result.length; // 元素个数
```

需要注意的是，返回的是 jQuery 对象，它类似数组，每个元素都是一个引用了 DOM 节点的对象。总之，jQuery 的选择器不会返回 undefined 或 null，因而使得你不必判断。

jQuery 对象和 DOM 对象之间可以互相转化：

```javascript
var div = $('#abc'); // jQuery对象
var divDom = div.get(0); // 假设存在div，获取第1个DOM元素
var another = $(divDom); // 重新把DOM包装为jQuery对象
```

通常情况下你不需要获取DOM对象，直接使用jQuery对象更加方便。如果你拿到了一个DOM对象，那可以简单地调用`$(aDomObject)`把它变成jQuery对象，这样就可以方便地使用jQuery的API了。

##### 按 TAG 查找

jQuery 中按 TAG 查找只需要写上 TAG 名称即可：

```javascript
$('tagName');
```

##### 按 class 查找

类似 CSS 中的选择，jQuery 中按 ID 查找可类似这样：

```javascript
$('.class');
```

##### 按属性查找

一个 DOM 节点除了`id`和`class`外还可以有很多属性，很多时候按属性查找会非常方便，比如在一个表单中按属性来查找：

```javascript
var email = $('[name=email]'); // 找出<??? name="email">
var passwordInput = $('[type=password]'); // 找出<??? type="password">
var a = $('[items="A B"]'); // 找出<??? items="A B">
var c = $('[name!=email]'); // 找出<??? name='email'> 以外的所有标签
```

当属性的值包含空格等特殊字符时，需要用双引号括起来。

按属性查找还可以使用前缀查找或者后缀查找：

```javascript
var icons = $('[name^=icon]'); // 找出所有name属性值以icon开头的DOM
// 例如: name="icon-1", name="icon-2"
var names = $('[name$=with]'); // 找出所有name属性值以with结尾的DOM
// 例如: name="startswith", name="endswith"
```

这个方法尤其适合通过 class 属性查找，且不受 class 包含多个名称的影响：

```javascript
var icons = $('[class^="icon-"]'); // 找出所有class包含至少一个以`icon-`开头的DOM
// 例如: class="icon-clock", class="abc icon-home"
```

##### 组合查找

组合查找就是把上述简单选择器组合起来使用。如果我们查找`$('[name=email]')`，很可能把表单外的`<div name="email">`也找出来，但我们只希望查找`<input>`，就可以这么写：

```javascript
var emailInput = $('input[name=email]'); // 不会找出<div name="email">
```

同样的，根据tag和class来组合查找也很常见：

```javascript
var tr = $('tr.red'); // 找出<tr class="red ...">...</tr>
```

##### 多项选择器

多项选择器就是把多个选择器用`,`组合起来一块选：

```
$('p,div'); // 把<p>和<div>都选出来
$('p.red,p.green'); // 把<p class="red">和<p class="green">都选出来
```

要注意的是，选出来的元素是**按照它们在 HTML 中出现的顺序排列的，而且不会有重复元素**。例如，`<p class="red green">`不会被上面的`$('p.red,p.green')`选择两次。

#### 层级选择器

因为 DOM 的结构就是层级结构，所以我们经常要根据层级关系进行选择。

##### 层级选择器

如果两个 DOM 元素具有层级关系（可以隔多层），就可以用`$('ancestor descendant')`来选择，层级之间用空格隔开。例如：

```html
<!-- HTML结构 -->
<div class="testing">
    <ul class="lang">
        <li class="lang-javascript">JavaScript</li>
        <li class="lang-python">Python</li>
        <li class="lang-lua">Lua</li>
    </ul>
</div>
```

要选出 JavaScript，可以用层级选择器：

```javascript
$('ul.lang li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
$('div.testing li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
```

因为`<div>`和`<ul>`都是`<li>`的祖先节点，所以上面两种方式都可以选出相应的`<li>`节点。

要选择所有的`<li>`节点，用：

```javascript
$('ul.lang li');
```

这种层级选择器相比单个的选择器好处在于，它缩小了选择范围，因为首先要定位父节点，才能选择相应的子节点，这样避免了页面其他不相关的元素。例如：

```javascript
$('form[name=upload] input');
```

就把选择范围限定在`name`属性为`upload`的表单里。如果页面有很多表单，其他表单的`<input>`不会被选择。

多层选择也是允许的：

```javascript
$('form.test p input'); // 在form表单选择被<p>包含的<input>
```

##### 子选择器

子选择器`$('parent>child')`类似层级选择器，但是限定了层级关系必须是父子关系，就是`<child>`节点必须是`<parent>`节点的直属子节点。还是以上面的例子：

```javascript
$('ul.lang>li.lang-javascript'); // 可以选出[<li class="lang-javascript">JavaScript</li>]
$('div.testing>li.lang-javascript'); // [], 无法选出，因为<div>和<li>不构成父子关系
```

##### 过滤器

过滤器一般不单独使用，它通常附加在选择器上，帮助我们更精确地定位元素。观察过滤器的效果：

```javascript
$('ul.lang li'); // 选出JavaScript、Python和Lua 3个节点

$('ul.lang li:first-child'); // 仅选出JavaScript
$('ul.lang li:last-child'); // 仅选出Lua
$('ul.lang li:nth-child(2)'); // 选出第N个元素，N从1开始
$('ul.lang li:nth-child(even)'); // 选出序号为偶数的元素
$('ul.lang li:nth-child(odd)'); // 选出序号为奇数的元素
```

##### 表单相关

针对表单元素，jQuery 还有一组特殊的选择器：

- `:input`：可以选择`<input>`，`<textarea>`，`<select>`和`<button>`；
- `:file`：可以选择`<input type="file">`，和`input[type=file]`一样；
- `:checkbox`：可以选择复选框，和`input[type=checkbox]`一样；
- `:radio`：可以选择单选框，和`input[type=radio]`一样；
- `:focus`：可以选择当前输入焦点的元素，例如把光标放到一个`<input>`上，用`$('input:focus')`就可以选出；
- `:checked`：选择当前勾上的单选框和复选框，用这个选择器可以立刻获得用户选择的项目，如`$('input[type=radio]:checked')`；
- `:enabled`：可以选择可以正常输入的`<input>`、`<select>` 等，也就是没有灰掉的输入；
- `:disabled`：和`:enabled`正好相反，选择那些不能输入的。

此外，jQuery 还有很多有用的选择器，例如，选出可见的或隐藏的元素：

```javascript
$('div:visible'); // 所有可见的div
$('div:hidden'); // 所有隐藏的div
```

### 查找和过滤

当我们拿到一个 jQuery 对象后，还可以以这个对象为基准，进行查找和过滤。

##### 查找

最常见的查找是在某个节点的所有子节点中查找，使用`find()`方法，它本身又接收一个任意的选择器。例如如下的 HTML 结构：

```html
<!-- HTML结构 -->
<ul class="lang">
    <li class="js dy">JavaScript</li>
    <li class="dy">Python</li>
    <li id="swift">Swift</li>
    <li class="dy">Scheme</li>
    <li name="haskell">Haskell</li>
</ul>
```

用`find()`查找：

```javascript
var ul = $('ul.lang'); // 获得<ul>
var dy = ul.find('.dy'); // 获得JavaScript, Python, Scheme
var swf = ul.find('#swift'); // 获得Swift
var hsk = ul.find('[name=haskell]'); // 获得Haskell
```

如果要从当前节点开始向上查找，使用`parent()`方法：

```javascript
var swf = $('#swift'); // 获得Swift
var parent = swf.parent(); // 获得Swift的上层节点<ul>
var a = swf.parent('.red'); // 获得Swift的上层节点<ul>，同时传入过滤条件。如果ul不符合条件，返回空jQuery对象
```

对于位于同一层级的节点，可以通过`next()`和`prev()`方法，例如：

当我们已经拿到`Swift`节点后：

```javascript
var swift = $('#swift');

swift.next(); // Scheme
swift.next('[name=haskell]'); // 空的jQuery对象，因为Swift的下一个元素Scheme不符合条件[name=haskell]

swift.prev(); // Python
swift.prev('.dy'); // Python，因为Python同时符合过滤器条件.dy
```

##### 过滤

和函数式编程的 map、filter 类似，jQuery 对象也有类似的方法。

`filter()`方法可以过滤掉不符合选择器条件的节点：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var a = langs.filter('.dy'); // 拿到JavaScript, Python, Scheme
```

或者传入一个函数（**不能用匿名函数**），要特别注意**函数内部的`this`被绑定为 DOM 对象，不是 jQuery 对象**：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
langs.filter(function () {
    return this.innerHTML.indexOf('S') === 0; // 返回S开头的节点
}); // 拿到Swift, Scheme
```

`map()`方法把一个 jQuery 对象包含的若干 DOM 节点转化为其他对象：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var arr = langs.map(function () {
    return this.innerHTML;
}).get(); // 用get()拿到包含string的Array：['JavaScript', 'Python', 'Swift', 'Scheme', 'Haskell']
```

此外，一个 jQuery 对象如果包含了不止一个 DOM 节点，`first()`、`last()`和`slice()`方法可以返回一个新的jQuery 对象，把不需要的 DOM 节点去掉：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var js = langs.first(); // JavaScript，相当于$('ul.lang li:first-child')
var haskell = langs.last(); // Haskell, 相当于$('ul.lang li:last-child')
var sub = langs.slice(2, 4); // Swift, Scheme, 参数和数组的slice()方法一致
```

### 操作 DOM

##### 修改 Text 和 HTML

jQuery 对象的`text()`和`html()`方法分别获取节点的文本和原始 HTML 文本，例如，如下的 HTML 结构：

```html
<!-- HTML结构 -->
<ul id="test-ul">
    <li class="js">JavaScript</li>
    <li name="book">Java &amp; JavaScript</li>
</ul>
```

分别获取文本和 HTML：

```javascript
$('#test-ul li[name=book]').text(); // 'Java & JavaScript'
$('#test-ul li[name=book]').html(); // 'Java &amp; JavaScript'
```

而要设置文本或 HTML，只需要在对应函数中加上参数即可：

```javascript
var j1 = $('#test-ul li.js');
var j2 = $('#test-ul li[name=book]');
j1.html('<span style="color: red">JavaScript</span>');
j2.text('JavaScript & ECMAScript');
```

一个 jQuery 对象可以包含 0 个或任意个 DOM 对象，**它的方法实际上会作用在对应的每个 DOM 节点上**。所以 jQuery 对象的另一个好处是我们可以执行一个操作，作用在对应的一组 DOM 节点上。即使选择器没有返回任何 DOM 节点，调用 jQuery 对象的方法仍然不会报错：

##### 修改 CSS

jQuery 对象的 `css('name', 'value')` 方法用于修改节点的 CSS 属性，其语法类似与：

```javascript
$('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');
```

*注意*，**jQuery 对象的所有方法都返回一个 jQuery 对象**（可能是新的也可能是自身），这样我们可以进行链式调用，非常方便。

`css()`方法将作用于 DOM 节点的`style`属性，具有最高优先级。如果要修改`class`属性，可以用 jQuery 提供的下列方法：

```javascript
var div = $('#test-div');
div.hasClass('highlight'); // false， class是否包含highlight
div.addClass('highlight'); // 添加highlight这个class
div.removeClass('highlight'); // 删除highlight这个clas
```

##### 显示和隐藏 DOM

要隐藏一个 DOM，我们可以设置 CSS 的`display`属性为`none`，利用`css()`方法就可以实现。不过，要显示这个 DOM 就需要恢复原有的`display`属性，这就得先记下来原有的`display`属性到底是`block`还是`inline`还是别的值。

考虑到显示和隐藏 DOM 元素使用非常普遍，jQuery 直接提供`show()`和`hide()`方法，我们不用关心它是如何修改`display`属性的，总之它能正常工作：

```javascript
var a = $('a[target=_blank]');
a.hide(); // 隐藏
a.show(); // 显示
```

##### 获取 DOM 信息

利用 jQuery 对象的若干方法，我们直接可以获取 DOM 的高宽等信息，而无需针对不同浏览器编写特定代码：

```javascript
// 浏览器可视窗口大小:
$(window).width(); // 800
$(window).height(); // 600

// HTML文档大小:
$(document).width(); // 800
$(document).height(); // 3500

// 某个div的大小:
var div = $('#test-div');
div.width(); // 600
div.height(); // 300
div.width(400); // 设置CSS属性 width: 400px，是否生效要看CSS是否有效
div.height('200px'); // 设置CSS属性 height: 200px，是否生效要看CSS是否有效
```

`attr()`和`removeAttr()`方法用于操作 DOM 节点的属性：

```javascript
// <div id="test-div" name="Test" start="1">...</div>
var div = $('#test-div');
div.attr('data'); // undefined, 属性不存在
div.attr('name'); // 'Test'
div.attr('name', 'Hello'); // div的name属性变为'Hello'
div.removeAttr('name'); // 删除name属性
div.attr('name'); // undefined
```

`prop()`方法和`attr()`类似，但是 HTML5 规定有一种属性在 DOM 节点中可以没有值，只有出现与不出现两种，例如：

```javascript
<input id="test-radio" type="radio" name="test" checked value="1">
```

等价于：

```javascript
<input id="test-radio" type="radio" name="test" checked="checked" value="1">
```

`attr()`和`prop()`对于属性`checked`处理有所不同：

```javascript
var radio = $('#test-radio');
radio.attr('checked'); // 'checked'
radio.prop('checked'); // true
```

`prop()`返回值更合理一些。不过，用`is()`方法判断更好：

```javascript
var radio = $('#test-radio');
radio.is(':checked'); // true
```

类似的属性还有`selected`，处理时最好用`is(':selected')`。

##### 操作表单

对于表单元素，jQuery 对象统一提供`val()`方法获取和设置对应的`value`属性：

```javascript
/*
    <input id="test-input" name="email" value="test">
    <select id="test-select" name="city">
        <option value="BJ" selected>Beijing</option>
        <option value="SH">Shanghai</option>
        <option value="SZ">Shenzhen</option>
    </select>
    <textarea id="test-textarea">Hello</textarea>
*/
var
    input = $('#test-input'),
    select = $('#test-select'),
    textarea = $('#test-textarea');

input.val(); // 'test'
input.val('abc@example.com'); // 文本框的内容已变为abc@example.com

select.val(); // 'BJ'
select.val('SH'); // 选择框已变为Shanghai

textarea.val(); // 'Hello'
textarea.val('Hi'); // 文本区域已更新为'Hi'
```

可见，一个`val()`就统一了各种输入框的取值和赋值的问题。

#### 修改 DOM 结构

##### 添加 DOM

要添加新的DOM节点，除了通过jQuery的`html()`这种暴力方法外，还可以用`append()`方法，例如：

```html
<div id="test-div">
    <ul>
        <li><span>JavaScript</span></li>
        <li><span>Python</span></li>
        <li><span>Swift</span></li>
    </ul>
</div>
```

如何向列表新增一个语言？首先要拿到`<ul>`节点：

```javascript
var ul = $('#test-div>ul');
```

然后，调用`append()`传入HTML片段：

```javascript
ul.append('<li><span>Haskell</span></li>');
```

除了接受字符串，`append()`还可以传入原始的 DOM 对象，jQuery 对象和函数对象：

```javascript
// 创建DOM对象:
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
// 添加DOM对象:
ul.append(ps);

// 添加jQuery对象:
ul.append($('#scheme'));

// 添加函数对象:
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
});
```

传入函数时，要求返回一个字符串、DOM 对象或者 jQuery 对象。因为 jQuery 的`append()`可能作用于一组 DOM 节点，只有传入函数才能针对每个 DOM 生成不同的子节点。

`append()`把 DOM 添加到最后，`prepend()`则把 DOM 添加到最前。

另外注意，如果要添加的 DOM 节点已经存在于 HTML文档中，它会首先从文档移除，然后再添加，也就是说，用`append()`，你可以移动一个 DOM 节点。

如果要把新节点插入到指定位置，例如，JavaScript 和 Python 之间，那么，可以先定位到 JavaScript，然后用`after()`方法：

```javascript
var js = $('#test-div>ul>li:first-child');
js.after('<li><span>Lua</span></li>');
```

也就是说，同级节点可以用`after()`或者`before()`方法。

##### 删除节点

要删除 DOM 节点，拿到 jQuery 对象后直接调用`remove()`方法就可以了。如果 jQuery 对象包含若干 DOM 节点，实际上可以一次删除多个 DOM 节点：

```javascript
var li = $('#test-div>ul>li');
li.remove(); // 所有<li>全被删除
```

### 事件

因为 JavaScript 在浏览器中以单线程模式运行，页面加载后，一旦页面上所有的 JavaScript 代码被执行完后，就只能依赖触发事件来执行 JavaScript 代码。

浏览器在接收到用户的鼠标或键盘输入后，会自动在对应的 DOM 节点上触发相应的事件。如果该节点已经绑定了对应的 JavaScript 处理函数，该函数就会自动调用。

由于不同的浏览器绑定事件的代码都不太一样，所以用 jQuery 来写代码，就屏蔽了不同浏览器的差异，我们总是编写相同的代码。

##### 绑定参数

举个例子，假设要在用户点击了超链接时弹出提示框，我们用 jQuery 这样绑定一个`click`事件：

```javascript
/* HTML:
 *
 * <a id="test-link" href="#0">点我试试</a>
 *
 */

// 获取超链接的jQuery对象:
var a = $('#test-link');
a.on('click', function () {
    alert('Hello!');
});
```

`on`方法用来绑定一个事件，我们需要传入事件名称和对应的处理函数。

另一种更简化的写法是直接调用`click()`方法：

```javascript
a.click(function () {
    alert('Hello!');
});
```

两者完全等价。我们通常用后面的写法。

jQuery 能够绑定的事件主要包括：

- 鼠标事件
  - click: 鼠标单击时触发；
  - dblclick：鼠标双击时触发；
  - mouseenter：鼠标进入时触发；
  - mouseleave：鼠标移出时触发；
  - mousemove：鼠标在 DOM 内部移动时触发；
  - hover：鼠标进入和退出时触发两个函数，相当于 mouseenter 加上 mouseleave。
- 键盘事件
  - 仅作用在当前焦点的 DOM 上，通常是`<input>`和`<textarea>`。
  - keydown：键盘按下时触发；
  - keyup：键盘松开时触发；
  - keypress：按一次键后触发。
- 其他事件
  - focus：当 DOM 获得焦点时触发；
  - blur：当 DOM 失去焦点时触发；
  - change：当`<input>`、`<select>`或`<textarea>`的内容改变时触发；
  - submit：当`<form>`提交时触发；
  - ready：当页面被载入并且 DOM 树完成初始化后触发。

其中，`ready`仅作用于`document`对象。由于`ready`事件在 DOM 完成初始化后触发，且只触发一次，所以非常适合用来写其他的初始化代码。

由于`ready`事件使用非常普遍，所以可以这样简化：

```javascript
$(document).ready(function () {
    // on('submit', function)也可以简化:
    $('#testForm).submit(function () {
        alert('submit!');
    });
});
```

甚至还可以再简化为：

```javascript
$(function () {
    // init...
});
```

上面的这种写法最为常见。如果你遇到`$(function () {...})`的形式，牢记这是`document`对象的`ready`事件处理函数。

完全可以反复绑定事件处理函数，它们会依次执行：

```javascript
$(function () {
    console.log('init A...');
});
$(function () {
    console.log('init B...');
});
$(function () {
    console.log('init C...');
});
```

##### 事件参数

有些事件，如`mousemove`和`keypress`，我们需要获取鼠标位置和按键的值，否则监听这些事件就没什么意义了。**所有事件都会传入`Event`对象作为参数**，可以从`Event`对象上获取到更多的信息：

```javascript
$(function () {
    $('#testMouseMoveDiv').mousemove(function (e) {
        $('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = ' + e.pageY);
    });
});
```

##### 取消绑定

个已被绑定的事件可以解除绑定，通过`off('click', function)`实现：

```javascript
function hello() {
    alert('hello!');
}

a.click(hello); // 绑定事件

// 10秒钟后解除绑定:
setTimeout(function () {
    a.off('click', hello);
}, 10000);
```

需要特别注意的是，下面这种写法是无效的：

```javascript
// 绑定事件:
a.click(function () {
    alert('hello!');
});

// 解除绑定:
a.off('click', function () {
    alert('hello!');
});
```

这是因为两个匿名函数虽然长得一模一样，但是它们是两个*不同*的函数对象，`off('click', function () {...})`无法移除已绑定的第一个匿名函数。

为了实现移除效果，可以使用`off('click')`一次性移除已绑定的`click`事件的所有处理函数。

同理，无参数调用`off()`一次性移除已绑定的所有类型的事件处理函数。

##### 事件触发条件

一个需要注意的问题是，事件的触发总是由用户操作引发的。例如，我们监控文本框的内容改动：

```javascript
var input = $('#test-input');
input.change(function () {
    console.log('changed...');
});
```

当用户在文本框中输入时，就会触发`change`事件。但是，如果用 JavaScript 代码去改动文本框的值，将*不会*触发`change`事件：

```javascript
var input = $('#test-input');
input.val('change it!'); // 无法触发change事件
```

有些时候，我们希望用代码触发`change`事件，可以直接调用无参数的`change()`方法来触发该事件：

```javascript
var input = $('#test-input');
input.val('change it!');
input.change(); // 触发change事件
```

`input.change()`相当于`input.trigger('change')`，它是`trigger()`方法的简写。

为什么我们希望手动触发一个事件呢？如果不这么做，很多时候，我们就得写两份一模一样的代码。

##### 浏览器安全限制

在浏览器中，有些 JavaScript 代码只有在用户触发下才能执行，例如，`window.open()`函数：

```javascript
// 无法弹出新窗口，将被浏览器屏蔽:
$(function () {
    window.open('/');
});
```

这些“敏感代码”只能由用户操作来触发：

```javascript
var button1 = $('#testPopupButton1');
var button2 = $('#testPopupButton2');

function popupTestWindow() {
    window.open('/');
}

button1.click(function () {
    popupTestWindow();
});

button2.click(function () {
    // 不立刻执行popupTestWindow()，3秒后执行:
    setTimeout(popupTestWindow, 3000);
});
```

当用户点击`button1`时，`click`事件被触发，由于`popupTestWindow()`在`click`事件处理函数内执行，这是浏览器允许的，而`button2`的`click`事件并未立刻执行`popupTestWindow()`，延迟执行的`popupTestWindow()`将被浏览器拦截。

```javascript
let checkboxs = $('form#test-form input[name=lang]');
let checkBtn = $('form#test-form label.selectAll>input');
let invertCheckBtn = $('form#test-form a');
checkB

let checkFunc = function(flag){
    checkboxs.prop('checked', flag);
    checkBtn.find('span')
}
let invertCheckFunc = function(){
    checkboxs.map(function(){
        $(this).prop('checked', !$(this).is(':checked'));
    });
}

checkBtn.click(function(){
    checkFunc(checkBtn.is(':checked'));
});
invertCheckBtn.click(invertCheckFunc);
```

### 动画

用 JavaScript 实现动画，原理很简单：我们只需要以固定的时间间隔修改元素的 CSS 样式，那便看起来像动画了。但这样手动实现比较麻烦，同时想复用代码的话则会更加复杂。

而使用 jQuery 实现动画，则非常简单，下面先看看 jQuery 内置的几种动画样式。

##### show / hide

直接以无参数形式调用`show()`和`hide()`，会显示和隐藏 DOM 元素。但是，只要传递一个时间参数进去，就变成了动画：

```javascript
var div = $('#test-show-hide');
div.hide(3000); // 在3秒钟内逐渐消失
```

时间以毫秒为单位，但也可以是`'slow'`，`'fast'`这些字符串：

```javascript
var div = $('#test-show-hide');
div.show('slow'); // 在0.6秒钟内逐渐显示
```

`toggle()`方法则根据当前状态决定是`show()`还是`hide()`。

##### slideUp / slideDown

`show()`和`hide()`是从左上角逐渐展开或收缩的，而`slideUp()`和`slideDown()`则是在垂直方向逐渐展开或收缩的。

`slideUp()`把一个可见的 DOM 元素收起来，效果跟拉上窗帘似的，`slideDown()`相反，而`slideToggle()`则根据元素是否可见来决定下一步动作：

```javascript
var div = $('#test-slide');
div.slideUp(3000); // 在3秒钟内逐渐向上消失
```

##### fadeIn / fadeOut

`fadeIn()`和`fadeOut()`的动画效果是淡入淡出，也就是通过不断设置 DOM 元素的`opacity`属性来实现，而`fadeToggle()`则根据元素是否可见来决定下一步动作：

```javascript
var div = $('#test-fade');
div.fadeOut('slow'); // 在0.6秒内淡出
```

##### 自定义动画 animate

`animate()`，它可以实现任意动画效果，我们需要传入的参数就是 DOM 元素最终的 CSS 状态和时间，jQuery 在时间段内不断调整 CSS 直到达到我们设定的值：

```javascript
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000); // 在3秒钟内CSS过渡到设定值
```

`animate()`还可以再传入一个函数，当动画结束时，该函数将被调用：

```javascript
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000, function () {
    console.log('动画已结束');
    // 恢复至初始状态:
    $(this).css('opacity', '1.0').css('width', '128px').css('height', '128px');
});
```

实际上**这个回调函数参数对于基本动画也是适用的**。

有了`animate()`，你就可以实现各种自定义动画效果了。

##### 串行动画

jQuery 的动画效果还可以串行执行，通过`delay()`方法还可以实现暂停，这样，我们可以实现更复杂的动画效果，而代码却相当简单：

```javascript
var div = $('#test-animates');
// 动画效果：slideDown - 暂停 - 放大 - 暂停 - 缩小
div.slideDown(2000)
   .delay(1000)
   .animate({
       width: '256px',
       height: '256px'
   }, 2000)
   .delay(1000)
   .animate({
       width: '128px',
       height: '128px'
   }, 2000);
}
```

因为动画需要执行一段时间，所以 jQuery 必须不断返回新的Promise对象才能后续执行操作。简单地把动画封装在函数中是不够的。

##### 为什么有的动画没有效果

你可能会遇到，有的动画如`slideUp()`根本没有效果。这是因为 jQuery 动画的原理是逐渐改变 CSS 的值，如`height`从`100px`逐渐变为`0`。但是很多不是 block性质的 DOM 元素，对它们设置`height`根本就不起作用，所以动画也就没有效果。

此外，jQuery 也没有实现对`background-color`的动画效果，用`animate()`设置`background-color`也没有效果。这种情况下可以使用 CSS3 的`transition`实现动画效果。

### AJAX

用 jQuery 的相关对象来处理 AJAX，不但不需要考虑浏览器问题，代码也能大大简化。

##### ajax

jQuery 在全局对象`jQuery`（也就是`$`）绑定了`ajax()`函数，可以处理 AJAX 请求。`ajax(url, settings)`函数需要接收一个 URL 和一个可选的`settings`对象，常用的选项如下：

- async：是否异步执行 AJAX 请求，默认为`true`，千万不要指定为`false`；
- method：发送的 Method，缺省为`'GET'`，可指定为`'POST'`、`'PUT'`等；
- contentType：发送 POST 请求的格式，默认值为`'application/x-www-form-urlencoded; charset=UTF-8'`，也可以指定为`text/plain`、`application/json`；
- data：发送的数据，可以是字符串、数组或object。如果是 GET 请求，data 将被转换成 query 附加到 URL 上，如果是 POST 请求，根据 contentType 把data 序列化成合适的格式；
- headers：发送的额外的 HTTP 头，必须是一个 object；
- dataType：接收的数据格式，可以指定为`'html'`、`'xml'`、`'json'`、`'text'`等，缺省情况下根据响应的`Content-Type`猜测。

下面的例子发送一个 GET 请求，并返回一个 JSON 格式的数据：

```javascript
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
});
// 请求已经发送了
```

不过，如何用回调函数处理返回的数据和出错时的响应呢？

还记得 Promise 对象吗？jQuery 的 jqXHR 对象类似一个 Promise 对象，我们可以用链式写法来处理各种回调：

```javascript
function ajaxLog(s) {
    var txt = $('#test-response-text');
    txt.val(txt.val() + '\n' + s);
}

$('#test-response-text').val('');

var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
}).done(function (data) {
    ajaxLog('成功, 收到的数据: ' + JSON.stringify(data));
}).fail(function (xhr, status) {
    ajaxLog('失败: ' + xhr.status + ', 原因: ' + status);
}).always(function () {
    ajaxLog('请求完成: 无论成功或失败都会调用');
});
```

##### get

对常用的 AJAX 操作，jQuery 提供了一些辅助方法。由于 GET 请求最常见，所以 jQuery 提供了`get()`方法，可以这么写：

```javascript
var jqxhr = $.get('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```

第二个参数如果是 object，jQuery 自动把它变成 query string 然后加到 URL 后面，实际的URL是：

```markdown
/path/to/resource?name=Bob%20Lee&check=1
```

这样我们就不用关心如何用 URL 编码并构造一个 query string 了。

##### post

`post()`和`get()`类似，但是传入的第二个参数默认被序列化为`application/x-www-form-urlencoded`：

```javascript
var jqxhr = $.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```

实际构造的数据`name=Bob%20Lee&check=1`作为 POST 的 body 被发送。

##### getJSON

由于 JSON 用得越来越普遍，所以 jQuery 也提供了`getJSON()`方法来快速通过 GET 获取一个 JSON 对象：

```javascript
var jqxhr = $.getJSON('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
}).done(function (data) {
    // data已经被解析为JSON对象了
});
```

##### 安全限制

jQuery 的 AJAX 完全封装的是 JavaScript 的 AJAX 操作，所以它的安全限制和前面讲的用 JavaScript 写 AJAX 完全一样。

如果需要使用 JSONP，可以在`ajax()`中设置`jsonp: 'callback'`，让 jQuery 实现 JSONP 跨域加载数据，举个例子：

```javascript
function lookStock() {
    $.ajax({
        type: 'get',
        url: "http://api.money.126.net/data/feed/0000001,1399001",
      async: false,
      dataType: 'jsonp',
      jsonp: 'callback',
      jsonpCallback: 'test'});
}
```

### 拓展

当我们使用 jQuery 对象的方法时，由于 jQuery 对象可以操作一组 DOM，而且支持链式操作，所以用起来非常方便。

但是 jQuery 内置的方法永远不可能满足所有的需求。比如，我们想要高亮显示某些 DOM 元素，用 jQuery 可以这么实现：

```javascript
$('span.hl').css('backgroundColor', '#fffceb').css('color', '#d85030');

$('p a.hl').css('backgroundColor', '#fffceb').css('color', '#d85030');
```

总是写重复代码可不好，万一以后还要修改字体就更麻烦了，能不能统一起来，写个`highlight()`方法？

```javascript
$('span.hl').highlight();

$('p a.hl').highlight();
```

答案是肯定的。我们可以扩展 jQuery 来实现自定义方法。将来如果要修改高亮的逻辑，只需修改一处扩展代码。这种方式也称为编写 jQuery 插件。

##### 编写 jQuery 插件

给 jQuery 对象绑定一个新方法是通过扩展`$.fn`对象实现的。让我们来编写第一个扩展——`highlight1()`：

```javascript
$.fn.highlight1 = function () {
    // this已绑定为当前jQuery对象:
    this.css('backgroundColor', '#fffceb').css('color', '#d85030');
    return this;
}
```

注意到函数内部的`this`在调用时被绑定为 jQuery 对象，所以函数内部代码可以正常调用所有 jQuery 对象的方法。

另外注意到代码第 4 行我们返回了 `this`，这么做是为了支持链式操作。而如果希望支持自定义参数，则对应函数可以这么写：

```javascript
$.fn.highlight2 = function (options) {
    // 要考虑到各种情况:
    // options为undefined
    // options只有部分key
    var bgcolor = options && options.backgroundColor || '#fffceb';
    var color = options && options.color || '#d85030';
    this.css('backgroundColor', bgcolor).css('color', color);
    return this;
}
```

对于默认值的处理，我们用了一个简单的`&&`和`||`短路操作符，总能得到一个有效的值。

另一种方法是使用 jQuery 提供的辅助方法`$.extend(target, obj1, obj2, ...)`，它把多个 object 对象的属性合并到第一个 target 对象中，遇到同名属性，总是使用靠后的对象的值，也就是**越往后优先级越高**：

```javascript
// 把默认值和用户传入的options合并到对象{}中并返回:
var opts = $.extend({}, {
    backgroundColor: '#00a8e6',
    color: '#ffffff'
}, options);
```

紧接着用户对`highlight2()`提出了意见：每次调用都需要传入自定义的设置，能不能让我自己设定一个缺省值，以后的调用统一使用无参数的`highlight2()`？

也就是说，我们设定的默认值应该能允许用户修改。

那默认值放哪比较合适？放全局变量肯定不合适，最佳地点是`$.fn.highlight2`这个函数对象本身。

于是最终版的`highlight()`终于诞生了：

```javascript
$.fn.highlight = function (options) {
    // 合并默认值和用户设定值:
    var opts = $.extend({}, $.fn.highlight.defaults, options);
    this.css('backgroundColor', opts.backgroundColor).css('color', opts.color);
    return this;
}

// 设定默认值:
$.fn.highlight.defaults = {
    color: '#d85030',
    backgroundColor: '#fff8de'
}
```

这次用户终于满意了。用户使用时，只需一次性设定默认值：

```javascript
$.fn.highlight.defaults.color = '#fff';
$.fn.highlight.defaults.backgroundColor = '#000';
```

然后就可以非常简单地调用`highlight()`了。

最终，我们得出编写一个 jQuery 插件的原则：

1. 给`$.fn`绑定函数，实现插件的代码逻辑；
2. 插件函数最后要`return this;`以支持链式调用；
3. 插件函数要有默认值，绑定在`$.fn.<pluginName>.defaults`上；
4. 用户在调用时可传入设定值以便覆盖默认值。

##### 针对特定元素的拓展

我们知道 jQuery 对象的有些方法只能作用在特定 DOM 元素上，比如`submit()`方法只能针对`form`。如果我们编写的扩展只能针对某些类型的 DOM 元素，应该怎么写？

还记得 jQuery 的选择器支持`filter()`方法来过滤吗？我们可以借助这个方法来实现针对特定元素的扩展。

举个例子，现在我们要给所有指向外链的超链接加上跳转提示，怎么做？

先写出用户调用的代码：

```javascript
$('#main a').external();
```

然后按照上面的方法编写一个`external`扩展：

```javascript
$.fn.external = function () {
    // return返回的each()返回结果，支持链式调用:
    return this.filter('a').each(function () {
        // 注意: each()内部的回调函数的this绑定为DOM本身!
        var a = $(this);
        var url = a.attr('href');
        if (url && (url.indexOf('http://')===0 || url.indexOf('https://')===0)) {
            a.attr('href', '#0')
             .removeAttr('target')
             .append(' <i class="uk-icon-external-link"></i>')
             .click(function () {
                if(confirm('你确定要前往' + url + '？')) {
                    window.open(url);
                }
            });
        }
    });
}
```

## underscore

前面我们已经讲过了，JavaScript 是函数式编程语言，支持高阶函数和闭包。函数式编程非常强大，可以写出非常简洁的代码。例如`Array`的`map()`和`filter()`方法。但`Array`有`map()`和`filter()`方法，可是 Object 没有这些方法。此外，低版本的浏览器例如 IE6～8也没有这些方法，怎么办？

方法一，自己把这些方法添加到`Array.prototype`中，然后给`Object.prototype`也加上`mapObject()`等类似的方法。

方法二，直接找一个成熟可靠的第三方开源库，使用统一的函数来实现`map()`、`filter()`这些操作。

我们采用方法二，选择的第三方库就是 underscore。

正如 jQuery 统一了不同浏览器之间的 DOM 操作的差异，让我们可以简单地对 DOM 进行操作，underscore 则提供了一套完善的函数式编程的接口，让我们更方便地在 JavaScript 中实现函数式编程。

jQuery 在加载时，会把自身绑定到唯一的全局变量`$`上，underscore 与其类似，会把自身绑定到唯一的全局变量`_`上，这也是为啥它的名字叫 underscore 的原因。

用 underscore 实现`map()`操作如下：

```javascript
_.map([1, 2, 3], (x) => x * x); // [1, 4, 9]
```

咋一看比直接用`Array.map()`要麻烦一点，可是 underscore 的`map()`还可以作用于 Object：

```javascript
_.map({ a: 1, b: 2, c: 3 }, (v, k) => k + '=' + v); // ['a=1', 'b=2', 'c=3']
```

### Collections

underscore 为集合类对象提供了一致的接口。集合类是指 Array 和 Object，暂不支持 Map 和 Set。

##### map / filter

和`Array`的`map()`与`filter()`类似，但是 underscore 的`map()`和`filter()`可以作用于 Object。当作用于 Object 时，传入的函数为`function (value, key)`，第一个参数接收 value，第二个参数接收 key：

```javascript
_.map(xxx, (v, k) => {xxx;}); // 返回一个Array，k可省略
_.mapObject(xxx, (v, k) => {xxx;}); // 返回一个Object,k可省略
```

##### every / some

当集合的所有元素都满足条件时，`_.every()`函数返回`true`，当集合的至少一个元素满足条件时，`_.some()`函数返回`true`，比如：

```javascript
// 所有元素都大于0？
_.every([1, 4, 7, -3, -9], (x) => x > 0); // false
// 至少一个元素大于0？
_.some([1, 4, 7, -3, -9], (x) => x > 0); // true
```

当集合是 Object 时，我们可以同时传入 key  和 value，比如：

```javascript
_.every({xxx}, (v, k) => {xxx}); // k同样可以省略，下同
_.some({xxx}, (v, k) => {xxx});
```

##### max / min

这两个函数直接返回集合中最大和最小的数：

```javascript
var arr = [3, 5, 7, 9];
_.max(arr); // 9
_.min(arr); // 3

// 空集合会返回-Infinity和Infinity，所以要先判断集合不为空：
_.max([])
-Infinity
_.min([])
Infinity
```

注意，如果集合是 Object，`max()`和`min()`只作用于 value，忽略掉 key：

```javascript
_.max({ a: 1, b: 2, c: 3 }); // 3
```

##### groupBy

`groupBy()`把集合的元素按照 key 归类，key 由传入的函数返回：

```javascript
var scores = [20, 81, 75, 40, 91, 59, 77, 66, 72, 88, 99];
var groups = _.groupBy(scores, function (x) {
    if (x < 60) {
        return 'C';
    } else if (x < 80) {
        return 'B';
    } else {
        return 'A';
    }
});
// 结果:
// {
//   A: [81, 91, 88, 99],
//   B: [75, 77, 66, 72],
//   C: [20, 40, 59]
// }
```

##### shuffle / sample

`shuffle()`用洗牌算法随机打乱一个集合：

```javascript
// 注意每次结果都不一样：
_.shuffle([1, 2, 3, 4, 5, 6]); // [3, 5, 4, 6, 2, 1]
```

`sample()`则是随机选择一个或多个元素：

```javascript
// 注意每次结果都不一样：
// 随机选1个：
_.sample([1, 2, 3, 4, 5, 6]); // 2
// 随机选3个：
_.sample([1, 2, 3, 4, 5, 6], 3); // [6, 1, 4]
```

更多完整的函数请参考 [underscore的文档](http://underscorejs.org/#collections)。

### Arrays

##### first / last

顾名思义，获取第一个和最后一个元素：

```javascript
_.first(arr);
_.last(arr);
```

##### flatten

`flatten()`接收一个`Array`，无论这个`Array`里面嵌套了多少个`Array`，`flatten()`最后都把它们变成一个一维数组：

```javascript
_.flatten([1, [2], [3, [[4], [5]]]]); // [1, 2, 3, 4, 5]
```

##### zip / unzip

`zip()`把**两个或多个数组**的所有元素按索引对齐，然后按索引合并成新数组，`unzip` 则相反，比如：

```javascript
let names = ['bob', 'cat'];
let genders = ['m', 'f'];
let nameGenders = _.zip(names, genders); // [['bob', 'm'], ['cat', 'f']]
[name, gender] = _.unzip(nameGenders); // [['bob', 'cat'], ['m', 'f']]
```

##### object

类似 `zip`，`object` 用于将传入两个数组元素按索引构造 Object，比如：

```javascript
var names = ['Adam', 'Lisa', 'Bart'];
var scores = [85, 92, 59];
_.object(names, scores);
// {Adam: 85, Lisa: 92, Bart: 59}
```

##### range

`range()`让你快速生成一个序列，不再需要用`for`循环实现了：

```javascript
// 从0开始小于10:
_.range(10); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

// 从1开始小于11：
_.range(1, 11); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// 从0开始小于30，步长5:
_.range(0, 30, 5); // [0, 5, 10, 15, 20, 25]

// 从0开始大于-10，步长-1:
_.range(0, -10, -1); // [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
```

更多完整的函数请参考 [underscore的文档](http://underscorejs.org/#arrays)。

### Functions

##### bind

`bind()`有什么用？我们先看一个常见的错误用法：

```javascript
var s = ' Hello  ';
s.trim();
// 输出'Hello'

var fn = s.trim;
fn();
// Uncaught TypeError: String.prototype.trim called on null or undefined
```

如果你想用`fn()`取代`s.trim()`，按照上面的做法是不行的，因为直接调用`fn()`传入的`this`指针是`undefined`，必须这么用：

```javascript
var s = ' Hello  ';
var fn = s.trim;
// 调用call并传入s对象作为this:
fn.call(s)
// 输出Hello
```

这样搞多麻烦！还不如直接用`s.trim()`。但是，`bind()`可以帮我们把`s`对象直接绑定在`fn()`的`this`指针上，以后调用`fn()`就可以直接正常调用了：

```javascript
var s = ' Hello  ';
var fn = _.bind(s.trim, s);
fn();
// 输出Hello
```

结论：当用一个变量`fn`指向一个对象的方法时，直接调用`fn()`是不行的，因为丢失了`this`对象的引用。用`bind`可以修复这个问题。

##### partial

`partial()`就是为一个函数创建偏函数。偏函数是什么东东？看例子：

假设我们要计算 $x^y$，这时只需要调用`Math.pow(x, y)`就可以了。

假设我们经常计算 $2^y$，每次都写`Math.pow(2, y)`就比较麻烦，如果创建一个新的函数能直接这样写`pow2N(y)`就好了，这个新函数`pow2N(y)`就是根据`Math.pow(x, y)`创建出来的偏函数，它固定住了原函数的第一个参数（始终为2）：

```
var pow2N = _.partial(Math.pow, 2);
pow2N(3); // 8
pow2N(5); // 32
pow2N(10); // 1024
```

如果我们不想固定第一个参数，想固定第二个参数怎么办？比如，希望创建一个偏函数`cube(x)`，计算$x^3$，可以用`_`作占位符，固定住第二个参数：

```
'use strict';

var cube = _.partial(Math.pow, _, 3);
cube(3); // 27
cube(5); // 125
cube(10); // 1000
```

可见，创建偏函数的目的是将原函数的某些参数固定住，可以降低新函数调用的难度。

##### memoize

如果一个函数调用开销很大，我们就可能希望能把结果缓存下来，以便后续调用时直接获得结果。举个例子，计算阶乘就比较耗时：

```javascript
function factorial(n) {
    console.log('start calculate ' + n + '!...');
    var s = 1, i = n;
    while (i > 1) {
        s = s * i;
        i --;
    }
    console.log(n + '! = ' + s);
    return s;
}

factorial(10); // 3628800
// 注意控制台输出:
// start calculate 10!...
// 10! = 3628800
```

用`memoize()`就可以自动缓存函数计算的结果：

```javascript
var factorial = _.memoize(function(n) {
    console.log('start calculate ' + n + '!...');
    var s = 1, i = n;
    while (i > 1) {
        s = s * i;
        i --;
    }
    console.log(n + '! = ' + s);
    return s;
});

// 第一次调用:
factorial(10); // 3628800
// 注意控制台输出:
// start calculate 10!...
// 10! = 3628800

// 第二次调用:
factorial(10); // 3628800
// 控制台没有输出
```

对于相同的调用，比如连续两次调用`factorial(10)`，第二次调用并没有计算，而是直接返回上次计算后缓存的结果。不过，当你计算`factorial(9)`的时候，仍然会重新计算。

可以对`factorial()`进行改进，让其递归调用：

```javascript
var factorial = _.memoize(function(n) {
    console.log('start calculate ' + n + '!...');
    if (n < 2) {
        return 1;
    }
    return n * factorial(n - 1);
});

factorial(10); // 3628800
// 输出结果说明factorial(1)~factorial(10)都已经缓存了:
// start calculate 10!...
// start calculate 9!...
// start calculate 8!...
// start calculate 7!...
// start calculate 6!...
// start calculate 5!...
// start calculate 4!...
// start calculate 3!...
// start calculate 2!...
// start calculate 1!...

factorial(9); // 362880
// console无输出
```

##### once

`once()`保证某个函数执行且仅执行一次。

##### delay

`delay()`可以让一个函数延迟执行，效果和`setTimeout()`是一样的，但是代码明显简单了：

```javascript
// 2秒后调用alert():
_.delay(alert, 2000);
```

如果要延迟调用的函数有参数，把参数也传进去：

```javascript
var log = _.bind(console.log, console);
_.delay(log, 2000, 'Hello,', 'world!');
// 2秒后打印'Hello, world!':
```

更多完整的函数请参考 [underscore的文档](http://underscorejs.org/#functions)。

### Objects

##### keys / allKeys

`keys()`可以非常方便地返回一个object自身所有的key，但不包含从原型链继承下来的，相反 `allKeys()` 则包含：

```javascript
function Student(name, age) {
    this.name = name;
    this.age = age;
}

var xiaoming = new Student('小明', 20);
_.keys(xiaoming); // ['name', 'age']

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype.school = 'No.1 Middle School';
var xiaoming = new Student('小明', 20);
_.allKeys(xiaoming); // ['name', 'age', 'school']
```

##### values

`values()`返回object自身但不包含原型链继承的所有值：

```javascript
var obj = {
    name: '小明',
    age: 20
};

_.values(obj); // ['小明', 20]
```

注意，没有`allValues()`，原因我也不知道。

##### mapObject

`mapObject()`就是针对 object 的 map 版本：

```javascript
var obj = { a: 1, b: 2, c: 3 };
// 注意传入的函数签名，value在前，key在后:
_.mapObject(obj, (v, k) => 100 + v); // { a: 101, b: 102, c: 103 }
```

##### invert

`invert()`把 object 的每个 key-value 来个交换，key 变成 value，value 变成 key：

```javascript
var obj = {
    Adam: 90,
    Lisa: 85,
    Bart: 59
};
_.invert(obj); // { '59': 'Bart', '85': 'Lisa', '90': 'Adam' }
```

##### extend / extendOwn

`extend()`把多个 object 的 key-value 合并到第一个 object 并返回：

```javascript
var a = {name: 'Bob', age: 20};
_.extend(a, {age: 15}, {age: 88, city: 'Beijing'}); // {name: 'Bob', age: 88, city: 'Beijing'}
// 变量a的内容也改变了：
a; // {name: 'Bob', age: 88, city: 'Beijing'}
```

注意：**如果有相同的 key，后面的 object 的 value 将覆盖前面的 object 的 value**。

`extendOwn()`和`extend()`类似，但获取属性时忽略从原型链继承下来的属性。

##### clone

如果我们要复制一个object对象，就可以用`clone()`方法，它会把原有对象的所有属性都复制到新的对象中：

```javascript
let source = {xxx};
let cloned = _.clone(source);
```

注意，**`clone()`是“浅复制”**。所谓“浅复制”就是说，两个对象相同的 key 所引用的 value 其实是同一对象。

##### isEqual

`isEqual()`对两个 object 进行深度比较，如果内容完全相同，则返回`true`：

```javascript
var o1 = { name: 'Bob', skills: { Java: 90, JavaScript: 99 }};
var o2 = { name: 'Bob', skills: { JavaScript: 99, Java: 90 }};

o1 === o2; // false
_.isEqual(o1, o2); // true
```

`isEqual()`其实对`Array`也可以比较：

```javascript
var o1 = ['Bob', { skills: ['Java', 'JavaScript'] }];
var o2 = ['Bob', { skills: ['Java', 'JavaScript'] }];

o1 === o2; // false
_.isEqual(o1, o2); // true
```

更多完整的函数请参考[underscore 的文档](http://underscorejs.org/#objects)。

### Chaining

类似 jQuery 支持的链式调用，underscore 提供了把对象包装成能进行链式调用的方法，就是`chain()`函数：

```javascript
var r = _.chain([1, 4, 9, 16, 25])
         .map(Math.sqrt)
         .filter(x => x % 2 === 1)
         .value(); // [1, 3, 5]
```

因为每一步返回的都是包装对象，所以最后一步的结果需要调用`value()`获得最终结果。

## Node.js

Node.js 是 JavaScript 的宿主环境之一（比如浏览器也是 JavaScript 的宿主环境）。要进入 Node.js 的交互环境，在命令行中输入 `node` 即可，要退出，则按两次 `ctrl + c`  或一次 `ctrl + d` 或输入 `.exit`。在 Node.js 交互环境下你可以输入任意 JavaScript 环境。

##### npm

npm 其实是 Node.js 的包管理工具（package manager）。而 npm 已经在 Node.js 安装的时候顺带装好了。我们在命令提示符或者终端输入`npm -v`，应该看到类似的输出：

```bash
C:\Users\wyx>npm -v
6.14.8
```

要更新各平台的 npm 或 node 版本，直接删除旧的装新的就好。

##### 第一个 Node 程序

假设你有一个 js 文件 first.js，那么通过 `node first.js` 即可通过 Node.js 执行对应程序。

另外注意通过 `node xxx.js` 执行和直接在 node 交互环境下执行命令是有一定区别的，直接执行文件时若想看到输出必须通过 `console.log` 显式地打印出来才能看到。

如果期望默认启用严格模式，那么可以给 Node.js 传递一个参数，即 `node --use_strict xxx.js`。

### 模块

在Node环境中，一个 `.js` 文件就称之为一个模块（module），模块的名称就是文件名（去掉 `.js` 后缀）。而要在其他 `js` 文件中引入模块，则需要调用 Node 的 `require` 函数，但对应模块需要先通过 `module.exports` 对对应部分进行导出。比如：

```javascript
// hello.js
function hello = name => console.log(`hello, ${name}!`);
module.exports = hello;

// greet.js
let greet = require('./hello');
greet('world');
```

引入的模块作为变量保存在`greet`变量中，其实质就是在 `hello.js` 中用 `module.exports` 导出的 `hello` 函数。

在使用 `require()` 引入模块的时候，请注意模块的相对路径，上面的例子适用于两个文件位于同一目录下，因此用了当前目录 `.`，而如果只写模块名，即：

```javascript
let greet = require('hello');
```

则 Node 会依次在内置模块、全局模块和当前模块下查找`hello.js`。

##### CommonJS 规范

这种模块加载机制被称为 CommonJS 规范。在这个规范下，每个`.js`文件都是一个模块，它们内部各自使用的变量名和函数名都互不冲突，例如，`hello.js`和`main.js`都申明了全局变量`var s = 'xxx'`，但互不影响。

一个模块想要对外暴露变量（函数也是变量），可以用`module.exports = variable;`，一个模块要引用其他模块暴露的变量，用`var ref = require('module_name');`就拿到了引用模块的变量。

##### 深入了解模块原理

首先对于不同模块中可以定义相同命名的全局变量，Node 是利用了 JavaScript 的函数式编程的特性，通过类似闭包方式实现了模块的隔离，比如某个 `hello.js`，其在 Node.js 加载后，可以类似包装成下面这样：

```javascript
// hello.js
let name = 'world';
console.log(`hello, ${name}!`);

// Node.js 加载后的 hello.js
(function(){
    let name = 'world';
    console.log(`hello, ${name}!`);
})();
```

但模块的 `module.exports` 是如何实现的？这个方法实现也相对容易，Node 可以先准备一个对象 `module`：

```javascript
// 准备module对象:
var module = {
    id: 'hello',
    exports: {}
};
var load = function (module) {
    // 读取的hello.js代码:
    function greet(name) {
        console.log('Hello, ' + name + '!');
    }
    
    module.exports = greet;
    // hello.js代码结束
    return module.exports;
};
var exported = load(module);
// 保存module:
save(module, exported);
```

可见，变量`module`是 Node 在加载 js 文件前准备的一个变量，并将其传入加载函数，我们在`hello.js`中可以直接使用变量`module`原因就在于它实际上是函数的一个参数：

```javascript
module.exports = greet;
```

通过把参数`module`传递给`load()`函数，`hello.js`就顺利地把一个变量传递给了 Node 执行环境，Node 会把`module`变量保存到某个地方。

由于 Node 保存了所有导入的`module`，当我们用`require()`获取 module 时，Node 找到对应的`module`，把这个`module`的`exports`变量返回，这样，另一个模块就顺利拿到了模块的输出：

```javascript
var greet = require('./hello');
```

以上是 Node 实现 JavaScript 模块的一个简单的原理介绍。

##### module.exports Vs exports

很多时候，你会看到，在 Node 环境中，有两种方法可以在一个模块中输出变量：

方法一：对 module.exports 赋值：

```javascript
// hello.js

function hello() {
    console.log('Hello, world!');
}

function greet(name) {
    console.log('Hello, ' + name + '!');
}

module.exports = {
    hello: hello,
    greet: greet
};
```

方法二：直接使用 exports：

```javascript
exports.hello = hello;
exports.greet = greet;
```

但是你不可以直接对`exports`赋值：

```javascript
// 代码可以执行，但是模块并没有输出任何变量:
exports = {
    hello: hello,
    greet: greet
};
```

因为 `exports` 实质上是 Node 出于方便而对于 `module.exports` 的别名，也就是说，类似：

```javascript
let exports = module.exports;
```

它们两个在 Node 中都被初始化为一个空对象 `{}` ，但实质存储的还是存在 `module.exports` 上，因为如同先前所言，存储是通过 `load` 函数存储，而里面调的是 `module.exports`，故而你直接给 `exports` 赋值，`module.exports` 还是 `{}`。

##### 结论

如果要输出一个键值对象`{}`，可以利用`exports`这个已存在的空对象`{}`，并继续在上面添加新的键值；

如果要输出一个函数或数组，必须直接对`module.exports`对象赋值。

所以我们可以得出结论：直接对`module.exports`赋值，可以应对任何情况：

```javascript
module.exports = {
    foo: function () { return 'foo'; }
};
```

或者：

```javascript
module.exports = function () { return 'foo'; };
```

最终，我们强烈建议使用`module.exports = xxx`的方式来输出模块变量。

### 基本模块

因为 Node.js 是运行在服务区端的 JavaScript 环境，服务器程序和浏览器程序相比，最大的特点是没有浏览器的安全限制了，而且，服务器程序必须能接收网络请求，读写文件，处理二进制内容，所以，Node.js 内置的常用模块就是为了实现基本的服务器功能。这些模块在浏览器环境中是无法被执行的，因为它们的底层代码是用 C/C++ 在 Node.js 运行环境中实现的。

##### global

JavaScript 有且仅有一个全局对象，在浏览器中，叫`window`对象。而在 Node.js 环境中，它叫做 `global`，在进入 Node.js 交互环境后可以直接输入。

##### process

`process`也是 Node.js 提供的一个对象，它代表当前 Node.js 进程。通过`process`对象可以拿到许多有用信息：

```javascript
> process === global.process;
true
> process.version;
'v5.2.0'
> process.platform;
'darwin'
> process.arch;
'x64'
> process.cwd(); //返回当前工作目录
'/Users/michael'
> process.chdir('/private/tmp'); // 切换当前工作目录
undefined
> process.cwd();
'/private/tmp'
```

JavaScript 程序是由事件驱动执行的单线程模型，Node.js 也不例外。Node.js 不断执行响应事件的 JavaScript 函数，直到没有任何响应事件的函数可以执行时，Node.js 就退出了。

如果我们想要在下一次事件响应中执行代码，可以调用`process.nextTick()`：

```javascript
// test.js

// process.nextTick()将在下一轮事件循环中调用:
process.nextTick(function () {
    console.log('nextTick callback!');
});
console.log('nextTick was set!');
```

用 Node 执行上面的代码`node test.js`，你会看到，打印输出是：

```javascript
nextTick was set!
nextTick callback!
```

这说明传入`process.nextTick()`的函数不是立刻执行，而是要等到下一次事件循环。

Node.js 进程本身的事件就由`process`对象来处理。如果我们响应`exit`事件，就可以在程序即将退出时执行某个回调函数：

```javascript
// 程序即将退出时的回调函数:
process.on('exit', function (code) {
    console.log('about to exit with code: ' + code);
});
```

##### 判断 JavaScript 运行环境

常用的方式就是根据浏览器和 Node 环境提供的全局变量名称来判断：

```javascript
if (typeof(window) === 'undefined') {
    console.log('node.js');
} else {
    console.log('browser');
}
```

#### fs

Node.js 内置的`fs`模块就是文件系统模块，负责读写文件。

和所有其它 JavaScript 模块不同的是，`fs`模块同时提供了异步和同步的方法。

##### 异步读文件

按照JavaScript的标准，异步读取一个文本文件的代码如下：

```javascript
var fs = require('fs');

fs.readFile('sample.txt', 'utf-8', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
```

请注意，上面代码使用相对路径时 `sample.txt`文件必须在当前目录下（同时也可以使用绝对路径），且文件编码为`utf-8`。

异步读取时，传入的回调函数接收两个参数，当正常读取时，`err`参数为`null`，`data`参数为读取到的 String。当读取发生错误时，`err`参数代表一个错误对象，`data`为`undefined`。这也是 Node.js 标准的回调函数：第一个参数代表错误信息，第二个参数代表结果。后面我们还会经常编写这种回调函数。

而如果要读取二进制而非文本文件，则无需包含编码格式，同时回调函数的`data` 参数将返回一个 `Buffer` 对象，比如下面这段读取图片文件的代码：

```javascript
var fs = require('fs');

fs.readFile('sample.png', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
        console.log(data.length + ' bytes');
    }
});
```

`Buffer` 对象在 Node.js 中是一个包含零个或任意个字节的数组（**但不是 Array**）。它可以和 String 做转换，比如：

```javascript
// buffer -> string
let text = data.toString('utf-8');
```

或者把 String 转换成 Buffer：

```javascript
// string -> buffer
let buf = Buffer.from(text, 'utf-8');
```

##### 同步读文件

除了标准的异步读取模式外，`fs`也提供相应的同步读取函数。同步读取的函数和异步函数相比，多了一个`Sync`后缀，并且不接收回调函数，函数直接返回结果。

用`fs`模块同步读取一个文本文件的代码如下：

```javascript
var fs = require('fs');

var data = fs.readFileSync('sample.txt', 'utf-8');
console.log(data);
```

可见，原异步调用的回调函数的`data`被函数直接返回，函数名需要改为`readFileSync`，其它参数不变。

如果同步读取文件发生错误，则需要用`try...catch`捕获该错误：

```javascript
try {
    var data = fs.readFileSync('sample.txt', 'utf-8');
    console.log(data);
} catch (err) {
    // 出错了
}
```

##### 写文件

将数据写入文件是通过`fs.writeFile()`实现的：

```javascript
var fs = require('fs');

var data = 'Hello, Node.js';
fs.writeFile('output.txt', data, function (err) {
    if (err) {
        console.log(err);
    } else {
        console.log('ok.');
    }
});
```

`writeFile()`的参数依次为文件名、数据和回调函数。如果传入的数据是 String，默认按 UTF-8 编码写入文本文件，如果传入的参数是`Buffer`，则写入的是二进制文件。回调函数由于只关心成功与否，因此只需要一个`err`参数。

和`readFile()`类似，`writeFile()`也有一个同步方法，叫`writeFileSync()`：

```javascript
var fs = require('fs');

var data = 'Hello, Node.js';
fs.writeFileSync('output.txt', data);
```

##### stat

如果我们要获取文件大小，创建时间等信息，可以使用`fs.stat()`，它返回一个`Stat`对象，能告诉我们文件或目录的详细信息：

```javascript
var fs = require('fs');

fs.stat('sample.txt', function (err, stat) {
    if (err) {
        console.log(err);
    } else {
        // 是否是文件:
        console.log('isFile: ' + stat.isFile());
        // 是否是目录: 
        console.log('isDirectory: ' + stat.isDirectory());
        if (stat.isFile()) {
            // 文件大小: b
            console.log('size: ' + stat.size);
            // 创建时间, Date对象:
            console.log('birth time: ' + stat.birthtime);
            // 修改时间, Date对象:
            console.log('modified time: ' + stat.mtime);
        }
    }
});
```

##### 同步还是异步

由于 Node 环境执行的 JavaScript 代码是服务器端代码，所以，绝大部分需要在服务器运行期反复执行业务逻辑的代码，*必须使用异步代码*，否则，同步代码在执行时期，服务器将停止响应，因为 JavaScript 只有一个执行线程。

服务器启动时如果需要读取配置文件，或者结束时需要写入到状态文件时，可以使用同步代码，因为这些代码只在启动和结束时执行一次，不影响服务器正常运行时的异步执行。

#### stream

`stream`是 Node.js 提供的又一个仅在服务区端可用的模块，目的是支持“流”这种数据结构。

在 Node.js 中，流也是一个对象，我们只需要响应流的事件就可以了：`data`事件表示流的数据已经可以读取了，`end`事件表示这个流已经到末尾了，没有数据可以读取了，`error`事件表示出错了。

下面是一个从文件流读取文本内容的示例：

```javascript
var fs = require('fs');

// 打开一个流:
var rs = fs.createReadStream('sample.txt', 'utf-8');

rs.on('data', function (chunk) {
    console.log('DATA:')
    console.log(chunk);
});

rs.on('end', function () {
    console.log('END');
});

rs.on('error', function (err) {
    console.log('ERROR: ' + err);
});
```

要注意，`data`事件可能会有多次，每次传递的`chunk`是流的一部分数据。

要以流的形式写入文件，只需要不断调用`write()`方法，最后以`end()`结束：

```javascript
var fs = require('fs');

var ws1 = fs.createWriteStream('output1.txt', 'utf-8');
ws1.write('使用Stream写入文本数据...\n');
ws1.write('END.');
ws1.end();

var ws2 = fs.createWriteStream('output2.txt');
ws2.write(new Buffer('使用Stream写入二进制数据...\n', 'utf-8'));
ws2.write(new Buffer('END.', 'utf-8'));
ws2.end();
```

所有可以读取数据的流都继承自`stream.Readable`，所有可以写入的流都继承自`stream.Writable`。

##### pipe

在 Node.js 中，`pipe()` 函数用于实现一个管道，它可以将一个 `Readable` 流和一个 `Writable` 流串起来，所有的数据自动从 `Readable` 流进入 `Writable` 流，比如下面是一个复制文件的例程：

```javascript
var fs = require('fs');

var rs = fs.createReadStream('sample.txt');
var ws = fs.createWriteStream('copied.txt');

rs.pipe(ws);
```

默认情况下，当`Readable`流的数据读取完毕，`end`事件触发后，将自动关闭`Writable`流。如果我们不希望自动关闭`Writable`流，需要传入参数：

```javascript
readable.pipe(writable, { end: false });
```

#### http

##### http 服务器

要开发 HTTP 服务器程序，从头处理 TCP 连接，解析 HTTP 是不现实的。这些工作实际上已经由 Node.js 自带的`http`模块完成了。应用程序并不直接和 HTTP 协议打交道，而是操作`http`模块提供的`request`和`response`对象。

`request`对象封装了 HTTP 请求，我们调用`request`对象的属性和方法就可以拿到所有 HTTP 请求的信息；

`response`对象封装了 HTTP 响应，我们操作`response`对象的方法，就可以把 HTTP 响应返回给浏览器。

用 Node.js 实现一个 HTTP 服务器程序非常简单。我们来实现一个最简单的Web程序`hello.js`，它对于所有请求，都返回`Hello world!`：

```javascript
// 导入http模块:
var http = require('http');

// 创建http server，并传入回调函数:
var server = http.createServer(function (request, response) {
    // 回调函数接收request和response对象,
    // 获得HTTP请求的method和url:
    console.log(request.method + ': ' + request.url);
    // 将HTTP响应200写入response, 同时设置Content-Type: text/html:
    response.writeHead(200, {'Content-Type': 'text/html'});
    // 将HTTP响应的HTML内容写入response:
    response.end('<h1>Hello world!</h1>');
});

// 让服务器监听8080端口:
server.listen(8080);

console.log('Server is running at http://127.0.0.1:8080/');
```

在命令提示符下运行该程序，可以看到以下输出：

```bash
$ node hello.js 
Server is running at http://127.0.0.1:8080/
```

不要关闭命令提示符，直接打开浏览器输入`http://localhost:8080`，即可看到服务器响应的内容。

##### 文件服务器

如果期望拓展上述简易服务器为简易文件器，只需要解析 request.url 中的路径，并在本地中找到对应文件发送出去即可。解析 URL 需要使用 Node.js 提供的 url 模块，可以通过 `parse()` 将一个字符串解析为一个 URL 对象：

```javascript
let url = require('url');
console.log(url.parse('http://user:pass@host.com:8080/path/to/file?query=string#hash'));
/*
Url {
  protocol: 'http:',
  slashes: true,
  auth: 'user:pass',
  host: 'host.com:8080',
  port: '8080',
  hostname: 'host.com',
  hash: '#hash',
  search: '?query=string',
  query: 'query=string',
  pathname: '/path/to/file',
  path: '/path/to/file?query=string',
  href: 'http://user:pass@host.com:8080/path/to/file?query=string#hash' }
  */
```

处理本地文件目录需要使用 Node.js 提供的 `path` 模块，它可以方便地构建目录：

```javascript
let path = require('path');
let workDir = path.resolve('.');
// 类似 Python os.path.join
let filePath = path.join(workDir, 'pub', 'index.html');
```

最后，给出一个简单的文件服务器实现：

```javascript
var
    fs = require('fs'),
    url = require('url'),
    path = require('path'),
    http = require('http');

// 从命令行参数获取root目录，默认是当前目录:
var root = path.resolve(process.argv[2] || '.');

console.log('Static root dir: ' + root);

// 创建服务器:
var server = http.createServer(function (request, response) {
    // 获得URL的path，类似 '/css/bootstrap.css':
    var pathname = url.parse(request.url).pathname;
    // 获得对应的本地文件路径，类似 '/srv/www/css/bootstrap.css':
    var filepath = path.join(root, pathname);
    // 获取文件状态:
    fs.stat(filepath, function (err, stats) {
        if (!err && stats.isFile()) {
            // 没有出错并且文件存在:
            console.log('200 ' + request.url);
            // 发送200响应:
            response.writeHead(200);
            // 将文件流导向response:
            fs.createReadStream(filepath).pipe(response);
        } else {
            // 出错了或者文件不存在:
            console.log('404 ' + request.url);
            // 发送404响应:
            response.writeHead(404);
            response.end('404 Not Found');
        }
    });
});

server.listen(8080);

console.log('Server is running at http://127.0.0.1:8080/');
```

没有必要手动读取文件内容。由于`response`对象本身是一个`Writable Stream`，直接用`pipe()`方法就实现了自动读取文件内容并输出到 HTTP 响应。

#### crypto

crypto 模块的目的是为了提供通用的加密和哈希算法。用纯 JavaScript 代码实现这些功能不是不可能，但速度会非常慢。Nodejs 用 C/C++ 实现这些算法后，通过cypto 这个模块暴露为 JavaScript 接口，这样用起来方便，运行速度也快。

##### MD5 和 SHA1

```javascript
const crypto = require('crypto');

const hash = crypto.createHash('md5');

// 可任意多次调用update():
hash.update('Hello, world!');
hash.update('Hello, nodejs!')
```

`update()`方法默认字符串编码为`UTF-8`，也可以传入 Buffer。

如果要计算SHA1，只需要把`'md5'`改成`'sha1'` ，还可以使用更安全的`sha256`和`sha512`。

##### Hmac

Hmac 算法也是一种哈希算法，它可以利用 MD5 或 SHA1 等哈希算法。不同的是，Hmac 还需要一个密钥：

```javascript
const crypto = require('crypto');

const hmac = crypto.createHmac('sha256', 'secret-key');

hmac.update('Hello, world!');
hmac.update('Hello, nodejs!');

console.log(hmac.digest('hex')); // 80f7e22570...
```

只要密钥发生了变化，那么同样的输入数据也会得到不同的签名，因此，可以把 Hmac 理解为用随机数“增强”的哈希算法。

##### AES

AES是一种常用的对称加密算法，加解密都用同一个密钥。crypto模块提供了AES支持，但是需要自己封装好函数，便于使用：

```javascript
const crypto = require('crypto');

function aesEncrypt(data, key) {
    const cipher = crypto.createCipher('aes192', key);
    var crypted = cipher.update(data, 'utf8', 'hex');
    crypted += cipher.final('hex');
    return crypted;
}

function aesDecrypt(encrypted, key) {
    const decipher = crypto.createDecipher('aes192', key);
    var decrypted = decipher.update(encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
}

var data = 'Hello, this is a secret message!';
var key = 'Password!';
var encrypted = aesEncrypt(data, key);
var decrypted = aesDecrypt(encrypted, key);

console.log('Plain text: ' + data); // Plain text: Hello, this is a secret message!
console.log('Encrypted text: ' + encrypted); // Encrypted text: 8a944d97bdabc157a5b7a40cb180e7...
console.log('Decrypted text: ' + decrypted); // Decrypted text: Hello, this is a secret message!
```

注意到 AES 有很多不同的算法，如`aes192`，`aes-128-ecb`，`aes-256-cbc`等，AES 除了密钥外还可以指定 IV（Initial Vector），不同的系统只要 IV 不同，用相同的密钥加密相同的数据得到的加密结果也是不同的。加密结果通常有两种表示方法：hex 和 base64，这些功能Nodejs全部都支持，但是在应用中要注意，如果加解密双方一方用 Nodejs，另一方用 Java、PHP 等其它语言，需要仔细测试。如果无法正确解密，要确认双方是否遵循同样的 AES 算法，字符串密钥和 IV 是否相同，加密后的数据是否统一为 hex 或 base64 格式。

##### Diffie-Hellman

DH 算法是一种密钥交换协议，它可以让双方在不泄漏密钥的情况下协商出一个密钥来。DH 算法基于数学原理，比如小明和小红想要协商一个密钥，可以这么做：

1. 小明先选一个素数和一个底数，例如，素数`p=23`，底数`g=5`（底数可以任选），再选择一个秘密整数`a=6`，计算`A=g^a mod p=8`，然后大声告诉小红：`p=23，g=5，A=8`；
2. 小红收到小明发来的`p`，`g`，`A`后，也选一个秘密整数`b=15`，然后计算`B=g^b mod p=19`，并大声告诉小明：`B=19`；
3. 小明自己计算出`s=B^a mod p=2`，小红也自己计算出`s=A^b mod p=2`，因此，最终协商的密钥`s`为`2`。

在这个过程中，密钥`2`并不是小明告诉小红的，也不是小红告诉小明的，而是双方协商计算出来的。第三方只能知道`p=23`，`g=5`，`A=8`，`B=19`，由于不知道双方选的秘密整数`a=6`和`b=15`，因此无法计算出密钥`2`。

用 crypto 模块实现 DH 算法如下：

```javascript
const crypto = require('crypto');

// xiaoming's keys:
var ming = crypto.createDiffieHellman(512);
var ming_keys = ming.generateKeys();

var prime = ming.getPrime();
var generator = ming.getGenerator();

console.log('Prime: ' + prime.toString('hex')); // Prime: a8224c...deead3
console.log('Generator: ' + generator.toString('hex')); // Generator: 02

// xiaohong's keys:
var hong = crypto.createDiffieHellman(prime, generator);
var hong_keys = hong.generateKeys();

// exchange and generate secret:
var ming_secret = ming.computeSecret(hong_keys);
var hong_secret = hong.computeSecret(ming_keys);

// print secret:
console.log('Secret of Xiao Ming: ' + ming_secret.toString('hex')); // Secret of Xiao Ming: 695308...d519be
console.log('Secret of Xiao Hong: ' + hong_secret.toString('hex')); // Secret of Xiao Hong: 695308...d519be
```

注意每次输出都不一样，因为素数的选择是随机的。

##### RSA

首先，在命令行执行以下命令以生成一个RSA密钥对：

```bash
openssl genrsa -aes256 -out rsa-key.pem 2048
```

根据提示输入密码，这个密码是用来加密 RSA 密钥的，加密方式指定为 AES256，生成的 RSA 的密钥长度是 2048 位。执行成功后，我们获得了加密的`rsa-key.pem`文件。

第二步，通过上面的`rsa-key.pem`加密文件，我们可以导出原始的私钥，命令如下：

```bash
openssl rsa -in rsa-key.pem -outform PEM -out rsa-prv.pem
```

输入第一步的密码，我们获得了解密后的私钥。

类似的，我们用下面的命令导出原始的公钥：

```bash
openssl rsa -in rsa-key.pem -outform PEM -pubout -out rsa-pub.pem
```

这样，我们就准备好了原始私钥文件`rsa-prv.pem`和原始公钥文件`rsa-pub.pem`，编码格式均为 PEM。

下面，使用`crypto`模块提供的方法，即可实现非对称加解密。

首先，我们用私钥加密，公钥解密：

```javascript
const
    fs = require('fs'),
    crypto = require('crypto');

// 从文件加载key:
function loadKey(file) {
    // key实际上就是PEM编码的字符串:
    return fs.readFileSync(file, 'utf8');
}

let
    prvKey = loadKey('./rsa-prv.pem'),
    pubKey = loadKey('./rsa-pub.pem'),
    message = 'Hello, world!';

// 使用私钥加密:
let enc_by_prv = crypto.privateEncrypt(prvKey, Buffer.from(message, 'utf8'));
console.log('encrypted by private key: ' + enc_by_prv.toString('hex'));


let dec_by_pub = crypto.publicDecrypt(pubKey, enc_by_prv);
console.log('decrypted by public key: ' + dec_by_pub.toString('utf8'));
```

执行后，可以得到解密后的消息，与原始消息相同。

接下来我们使用公钥加密，私钥解密：

```javascript
// 使用公钥加密:
let enc_by_pub = crypto.publicEncrypt(pubKey, Buffer.from(message, 'utf8'));
console.log('encrypted by public key: ' + enc_by_pub.toString('hex'));

// 使用私钥解密:
let dec_by_prv = crypto.privateDecrypt(prvKey, enc_by_pub);
console.log('decrypted by private key: ' + dec_by_prv.toString('utf8'));
```

执行得到的解密后的消息仍与原始消息相同。

如果我们把`message`字符串的长度增加到很长，例如 1M，这时，执行 RSA 加密会得到一个类似这样的错误：`data too large for key size`，这是因为 RSA 加密的原始信息必须小于 Key 的长度。那如何用 RSA 加密一个很长的消息呢？实际上，RSA 并不适合加密大数据，而是先生成一个随机的 AES 密码，用 AES 加密原始信息，然后用 RSA 加密 AES 口令，这样，实际使用 RSA 时，给对方传的密文分两部分，一部分是 AES 加密的密文，另一部分是 RSA 加密的 AES 口令。对方用 RSA 先解密出 AES 口令，再用 AES 解密密文，即可获得明文。

##### 证书

crypto 模块也可以处理数字证书。数字证书通常用在 SSL 连接，也就是 Web 的 https 连接。一般情况下，https 连接只需要处理服务器端的单向认证，如无特殊需求（例如自己作为 Root 给客户发认证证书），建议用反向代理服务器如 Nginx 等 Web 服务器去处理证书。

## Web 开发

Web 应用开发可以说是目前软件开发中最重要的部分。Web 开发也经历了好几个阶段：

静态 Web 页面：由文本编辑器直接编辑并生成静态的 HTML 页面，如果要修改 Web 页面的内容，就需要再次编辑 HTML 源文件，早期的互联网Web页面就是静态的；

CGI：由于静态 Web 页面无法与用户交互，比如用户填写了一个注册表单，静态Web页面就无法处理。要处理用户发送的动态数据，出现了Common Gateway Interface，简称 CGI，用 C/C++ 编写。

ASP/JSP/PHP：由于 Web 应用特点是修改频繁，用 C/C++ 这样的低级语言非常不适合 Web 开发，而脚本语言由于开发效率高，与 HTML 结合紧密，因此，迅速取代了 CGI 模式。ASP 是微软推出的用 VBScript 脚本编程的 Web 开发技术，而 JSP 用 Java 来编写脚本，PHP 本身则是开源的脚本语言。

MVC：为了解决直接用脚本语言嵌入 HTML 导致的可维护性差的问题，Web 应用也引入了 Model-View-Controller 的模式，来简化 Web 开发。ASP 发展为ASP.Net，JSP 和 PHP 也有一大堆 MVC 框架。

目前，Web 开发技术仍在快速发展中，异步开发、新的 MVVM 前端技术层出不穷。

由于 Node.js 把 JavaScript 引入了服务器端，因此，原来必须使用 PHP/Java/C#/Python/Ruby 等其他语言来开发服务器端程序，现在可以使用 Node.js 开发了！

用 Node.js 开发 Web 服务器端，有几个显著的优势：

一是后端语言也是 JavaScript，以前掌握了前端 JavaScript 的开发人员，现在可以同时编写后端代码；

二是前后端统一使用 JavaScript，就没有切换语言的障碍了；

三是速度快，非常快！这得益于 Node.js 天生是异步的。

在 Node.js 诞生后的短短几年里，出现了无数种 Web 框架、ORM 框架、模版引擎、测试框架、自动化构建工具，数量之多，即使是 JavaScript 老司机，也不免眼花缭乱。

常见的 Web 框架包括：[Express](http://expressjs.com/)，[Sails.js](http://sailsjs.org/)，[koa](http://koajs.com/)，[Meteor](https://www.meteor.com/)，[DerbyJS](http://derbyjs.com/)，[Total.js](https://www.totaljs.com/)，[restify](http://restify.com/)……

ORM 框架比 Web 框架要少一些：[Sequelize](http://www.sequelizejs.com/)，[ORM2](http://dresende.github.io/node-orm2/)，[Bookshelf.js](http://bookshelfjs.org/)，[Objection.js](http://vincit.github.io/objection.js/)……

模版引擎 PK：[Jade](http://jade-lang.com/)，[EJS](http://ejs.co/)，[Swig](https://github.com/paularmstrong/swig)，[Nunjucks](http://mozilla.github.io/nunjucks/)，[doT.js](http://olado.github.io/doT/)……

测试框架包括：[Mocha](http://mochajs.org/)，[Expresso](http://visionmedia.github.io/expresso/)，[Unit.js](http://unitjs.com/)，[Karma](http://karma-runner.github.io/)……

构建工具有：[Grunt](http://gruntjs.com/)，[Gulp](http://gulpjs.com/)，[Webpack](http://webpack.github.io/)……

目前，在 npm 上已发布的开源 Node.js 模块数量超过了 30 万个。




