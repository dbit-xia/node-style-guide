# Node.js Style Guide

这是一份关于如何写出一致且美观的 Node.js 代码的风格指南。
它从社区最流行的写法中抽取出来，同时融入了部分个人观点。

这份指南由 [Felix](http://felixge.de/) 编写。
通过 [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/) 发布。
你可以 fork 这份指南，并针对自己的需求的进行部分的调整。

中文指南由 [dead_horse](http://deadhorse.me) 翻译，同时根据个人和团队经验有部分增加条目。

![Creative Commons License](http://i.creativecommons.org/l/by-sa/3.0/88x31.png)

## 使用分号

是否使用分号，在社区争论已久。
isaac 也写过一篇讨论的[文章](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding)，
但是，当可以用廉价的语法来消除一些可能引入的错误的时候，请当一个保守派。

## 每行80个字符

限制你每行代码不超过80个字符。尽管现在的显示器越来越大，但是你的大脑并没有变大，并且你还可以把你的大显示器切分成多屏来显示。


## 使用单引号

除非编写.json文件，其他时候都请用单引号包裹字符串。

*Right:*

```js
let foo = 'bar';
```

*Wrong:*

```js
let foo = "bar";
```

## 大括号位置

请把你的所有的左大括号都放在语句开始的这一行。

*Right:*

```js
if (true) {
  console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
  console.log('losing');
}
```

同时，请注意在条件判断前后都添加一个空格。

## 每个变量声明都带一个 let

每个变量声明都带一个 let ，这样删除或者调整变量声明的顺序会更加容易。
不要把变量都声明在最前面，而是声明在它最有意义的地方。

*Right:*

```js
let keys   = ['foo', 'bar'];
let values = [23, 42];

let object = {};
while (items.length) {
  let key = keys.pop();
  object[key] = values.pop();
}
```

*Wrong:*

```js
let keys = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key;

while (items.length) {
  key = keys.pop();
  object[key] = values.pop();
}
```

## 变量、属性和函数名都采用小驼峰

变量、属性和函数的命名风格都需要遵循小驼峰风格。
同时所有的命名都是有意义的。
尽量避免用单字符变量和少见单词来命名。

*Right:*

```js
let adminUser = db.query('SELECT * FROM users ...');
```

*Wrong:*

```js
let admin_user = db.query('SELECT * FROM users ...');
let a = db.query('SELECT * FROM users ...');
```

## 类名采用大驼峰

类名都采用大驼峰风格来命名。

*Right:*

```js
function BankAccount() {
}
```

*Wrong:*

```js
function bank_Account() {
}
```

## 用大写来标识常量

常量变量和对象的静态常量属性都需要特殊表明，通过全部大写的方式来表明。

*Right:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## 对象、数组的创建

使用尾随逗号，尽量用一行来声明，只有在编译器不接受的情况下才把对象的 key 用单引号包裹。
使用字面表达式，用 `{}, []` 代替 `new Array, new Object`。

*Right:*

```js
let a = ['hello', 'world'];
let b = {
  good: 'code',
  'is generally': 'pretty',
};
```

*Wrong:*

```js
let a = [
  'hello', 'world'
];
let b = {"good": 'code'
        , is generally: 'pretty'
        };
```

## 使用 === 比较符

写代码并不是在背这些 [stupid rules][comparisonoperators] 。使用 `===` 操作符来进行比较操作，它会完全按照你的期望来执行。

*Right:*

```js
let a = 0;
if (a === '') {
  console.log('winning');
}

```

*Wrong:*

```js
let a = 0;
if (a == '') {
  console.log('losing');
}
```

[comparisonoperators]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Comparison_Operators

## 三元操作符分多行

三元操作符不应该写在一行，将它分割到多行。

*Right:*

```js
let foo = (a === b)
  ? 1
  : 2;
```

*Wrong:*

```js
let foo = (a === b) ? 1 : 2;
```

## 不要扩展内建类型

不要扩展 javascript 内建对象的方法。将来的你会感谢你这个做法的。

*Right:*

```js
let a = [];
if (!a.length) {
  console.log('winning');
}
```

*Wrong:*

```js
Array.prototype.empty = function() {
  return !this.length;
}

let a = [];
if (a.empty()) {
  console.log('losing');
}
```

## 使用有意义的判断条件

所有复杂的条件判断都需要赋予一个有意义的名字或者方法。

*Right:*

```js
let isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

if (isValidPassword) {
  console.log('winning');
}
```

*Wrong:*

```js
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
  console.log('losing');
}
```

## 写精简的函数

保持你的函数尽可能的精简。
一个好的函数应该能够在幻灯片上一屏显示，并且让坐在教室最后一排的人看清楚。
别再去数你的每一个函数并控制在15行以内了。

## 尽早的从函数中返回

为了避免深入嵌套的 if 语句，请尽早的从函数中返回。

*Right:*

```js
function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
```

*Wrong:*

```js
function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```

针对这个示例，甚至可以进一步精简优化：

```js
function isPercentage(val) {
  let isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

## 不要嵌套闭包

使用闭包，但是不要嵌套他们，否则你的代码将会一团糟。

*Right:*

```js
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```

*Wrong:*

```js
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```

## 使用单行注释风格

不管是单行注释还是多行注释，都使用 `//` 。
同时请尝试在更高层次来编写注释（解释函数整体的思路），
只在解释一些难以理解代码的时候添加注释，而不是给一些琐碎的东西加上注释。

*Right:*

```js
// 'ID_SOMETHING=VALUE' -> ['ID_SOMETHING=VALUE'', 'SOMETHING', 'VALUE']
let matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// This function has a nasty side effect where a failure to increment a
// redis counter used for statistics will cause an exception. This needs
// to be fixed in a later iteration.
function loadUser(id, cb) {
  // ...
}

let isSessionValid = (session.expires < Date.now());
if (isSessionValid) {
  // ...
}
```

*Wrong:*

```js
// Execute a regex
let matches = item.match(/ID_([^\n]+)=([^\n]+)/));

// Usage: loadUser(5, function() { ... })
function loadUser(id, cb) {
  // ...
}

// Check if the session is valid
let isSessionValid = (session.expires < Date.now());
// If the session is valid
if (isSessionValid) {
  // ...
}
```

## Object.freeze, Object.preventExtensions, Object.seal, with, eval

这一堆屎一样的东西，你永远都不会需要他们。

## Getters 和 Setters

不要使用 setters ，他们会引发一些使用你的代码的人无法解决的问题。
当没有[副作用][sideeffect]的时候，可以使用 getters，例如提供一个集合类的长度属性的时候。

[sideeffect]: http://en.wikipedia.org/wiki/Side_effect_(computer_science)

## 异步回调函数

Node 的异步回调函数的第一个参数应该是错误指示，只有这样才能够享受许多流程控制模块的福利。

*Right:*

```js
function cb(err, data , ...) {...}
```

*Wrong:*

```js
function cb(data, ...) {...}
```

## 继承

尽管有许多的方法来实现继承，但是最为推荐的是 Node 的标准写法：

```js
function Socket(options) {
  // ...
  stream.Stream.call(this);
  // ...
}

util.inherits(Socket, stream.Stream);
```

## 文件命名

单词之间使用 `_` underscore 来分割,或用小驼峰规范，如果你不想暴露某个文件给用户，你也可以用 `_` 来开头

类定义文件,可使用大写字母开头

*Right:*

```
child_process.js
string_decoder.js
_linklist.js
getUserName.js //单个方法
User.js //单纯类定义文件
```


## 空格

在所有的操作符前后都添加空格，`function` 关键字后面添加空格

*Right:*

```js
let add = function (a, b) {
  return a + b;
};
```

*Wrong:*

```js
let add=function(a,b){
  return a+b;
}
```

## 尽量参照 Node.js 源码的编码风格

* [node 源码](https://github.com/joyent/node)
* [Google’s JavaScript style guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)

## 入乡随俗

给别人的项目提交 pull request 的时候，要注意遵循项目的编码规范，保持项目编码风格的统一。
