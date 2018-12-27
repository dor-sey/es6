# 数组的解构赋值
## 1. 用法
* ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构

```
let [a, b, c] = [1, 2, 3]; //a=1 b=2 c=3
let [foo, [[bar], baz]] = [1, [[2], 3]]; // foo=1 bar=2 baz=3
let [ , , third] = ["foo", "bar", "baz"]; //third=baz
let [head, ...tail] = [1, 2, 3, 4]; // head=1 tail=[2,3,4]
let [x, y, ...z] = ['a']; x=a y=undefined z=[]
```

* 如果解构不成功，变量的值就等于undefined

```
let [foo] = []; //foo=undefined
let [bar, foo] = [1];  //foo=undefined
```

* 不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组

```
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

* 如果等号的右边是不可遍历的结构，会报错

```
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

## 2. 指定默认值
* 解构赋值允许指定默认值

```
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

* 只有当一个数组成员严格等于undefined，默认值才会生效。

```
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

* 如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。

```
function f() {
  console.log('aaa');
}

let [x = f()] = [1]; //因为x能取到值，函数f不会执行
```

* 默认值可以引用解构赋值的其他变量，但该变量必须已经声明。

```
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError: y is not defined
```

# 对象的解构赋值
* 变量必须与属性同名，才能取到正确的值

```
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```

* 对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

```
let { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };

let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'

let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```

* 解构也可以用于嵌套结构的对象

```
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p: [x, { y }] } = obj; //此时p是模式，不是变量，不会被赋值，
x // "Hello"
y // "World"

let { p, p: [x, { y }] } = obj; //此时，p会被赋值
x // "Hello"
y // "World"
p // ["Hello", {y: "World"}]
```
```
const node = {
  loc: {
    start: {
      line: 1,
      column: 5
    }
  }
};

let { loc, loc: { start }, loc: { start: { line }} } = node;
line // 1
loc  // Object {start: Object}
start // Object {line: 1, column: 5}

/*
 * 上面代码有三次解构赋值，分别是对loc、start、line三个属性的解构赋值。
 * 注意，最后一次对line属性的解构赋值之中，只有line是变量，loc和start都是模式，不是变量。
 */
```

* 指定默认值

```
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```

* 解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。

```
// 报错
let {foo: {bar}} = {baz: 'baz'};
```

* 将一个已经声明的变量用于解构赋值

```
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error

// 正确的写法
let x;
({x} = {x: 1});
```

* 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。

```
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```

# 字符串的解构赋值

```
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

```

* 对字符串的length属性解构

```
let {length : len} = 'hello';
len // 5
``` 

# 数值和布尔值的解构赋值
* 解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。

```
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

* 只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。

```
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

# 函数参数的解构赋值
## 1. 函数参数也可以进行解构赋值

```
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3

[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```

## 2. 函数参数的解构也可以使用默认值

```
function move({x = 0, y = 0} = {}) { 
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

# 圆括号问题
## 1. 不能使用圆括号的情况
* 变量声明语句

```
// 全部报错
let [(a)] = [1];

let {(x: c)} = {};
let {(x): c} = {};

let { o: ({ p: p }) } = { o: { p: 2 } };
```

* 函数参数

```
// 报错
function f([(z)]) { return z; }
// 报错
function f([z,(x)]) { return x; }
```

* 赋值语句的模式

```
// 全部报错
({ p: a }) = { p: 42 }; //将整个模式放在圆括号之中，导致报错
([a]) = [5]; //将整个模式放在圆括号之中，导致报错
// 报错
[({ p: a }), { x: c }] = [{}, {}]; //将一部分模式放在圆括号之中，导致报错。
```

# 可以使用圆括号的情况

```
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确

/*
* 上面三行语句都可以正确执行，因为首先它们都是赋值语句，而不是声明语句；
* 其次它们的圆括号都不属于模式的一部分。
* 第一行语句中，模式是取数组的第一个成员，跟圆括号无关；
* 第二行语句中，模式是p，而不是d；第三行语句与第一行语句的性质一致。
*/
```

# 用途
## 1. 交换变量的值

```
let x = 1;
let y = 2;

[x, y] = [y, x];
// x=2 y=1
```

## 2. 函数返回多个值

```
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

## 3. 函数参数的定义

```
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

## 4. 提取JSON数据

```
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

## 5. 函数的默认值

```
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};
```

## 6. 遍历MAP结构

```
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

## 7. 输入模块的指定方法

```
const { SourceMapConsumer, SourceNode } = require("source-map");
```