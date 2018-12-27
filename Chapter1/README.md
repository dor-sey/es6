# let命令
## 1. 不存在变量提升
```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```
## 2. 暂时性死区
只要块级作用域内存在let命令，它所声明的变量就“绑定”这个区域，不再受外部的影响。
```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError 在声明tmp之前调用tmp
  let tmp;
}

```

```
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错 x在y声明之前调用y
```
## 3. 不允许重复声明
let不允许在相同作用域内，重复声明同一个变量。
```
// 报错
function func() {
  let a = 10;
  var a = 1;
}

function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}
```
## 4. 作用域为块级作用域
* 块级作用域出现的原因
    * 内层变量可能会覆盖外层变量
    ```
    var tmp = new Date();
    function f() {
    console.log(tmp); //相同作用域，变量提升导致
    if (false) {
        var tmp = 'hello world'; 
    }
    }

    f(); // undefined
    ```
    * 用来计数的循环变量泄露为全局变量
    ```
    var s = 'hello';

    for (var i = 0; i < s.length; i++) {
    console.log(s[i]);
    }

    console.log(i); // 5 
    ```
* 块级作用域与函数声明
    * es5规定不允许在块级作用域声明函数，但浏览器为了兼容不做限制
    ```
    function f() { console.log('I am outside!'); }

    (function () {
    if (false) {
        // 重复声明一次函数f
        function f() { console.log('I am inside!'); }
    }

    f();
    }());
    //I am inside! 存在变量提升，块级作用域声明的函数会提升到函数头部
    ```
    * es6允许在块级作用域声明函数，函数声明语句的行为类似于let，在块级作用域外不可引用。
    ```
    // 浏览器的 ES6 环境
    function f() { console.log('I am outside!'); }

    (function () {
    if (false) {
        // 重复声明一次函数f
        function f() { console.log('I am inside!'); }
    }

    f();
    }());
    // I am outside! 在块级作用域声明函数类似于let,只在块级作用域有效
    ```
    * 为了兼容处理在浏览器的 ES6 环境中，块级作用域内声明的函数，行为类似于var声明的变量。
    ```
    // 浏览器的 ES6 环境
    function f() { console.log('I am outside!'); }

    (function () {
    if (false) {
        // 重复声明一次函数f
        function f() { console.log('I am inside!'); }
    }

    f();
    }());
    // Uncaught TypeError: f is not a function 相当于var f = fnction()，会提升到函数头部 var f; f未定义
    ```

# const命令
## 1. const用于声明常量
* const用于声明常量，一旦声明，常量的值就不能改变

```
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```

* const声明的变量不得改变值，必须立即初始化

```
const foo;
// SyntaxError: Missing initializer in const declaration
```

* const只是不允许变量指向的内存地址保存的数据不变，对于复杂类型只保证内存地址存放的指针不变

```
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

## 2. 作用域为块级作用域
```
if (true) {
  const MAX = 5;
}

MAX // Uncaught ReferenceError: MAX is not defined
```
## 3. 暂时性死区
```
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```
## 4. 不允许重复声明
```
var message = "Hello!";
let age = 25;

// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```

#es6声明变量的方式
ES5 只有两种声明变量的方法：var命令和function命令。
ES6 除了var命令和function命令，还有let和const命令，import命令和class命令。共六种。

# 顶层对象
let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性
```
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

# global 对象
## 1. 顶层对象不统一
* 浏览器里面，顶层对象是window，但 Node 和 Web Worker 没有window。
* 浏览器和 Web Worker 里面，self也指向顶层对象，但是 Node 没有self。
* Node 里面，顶层对象是global，但其他环境都不支持。
## 2. 兼容方法

```
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

## 3. 引入global作为顶层对象
现在有一个提案，在语言标准的层面，引入global作为顶层对象。也就是说，在所有环境下，global都是存在的，都可以从它拿到顶层对象。

```
//垫片库system.global模拟了这个提案，可以在所有环境拿到global。
// CommonJS 的写法
require('system.global/shim')();
// ES6 模块的写法
import shim from 'system.global/shim'; shim();
```