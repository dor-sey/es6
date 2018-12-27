# 变量解构赋值的用法
```
let [x, y] = [1, 2]
let {foo, bar} = { foo: 'a', bar: '2' }
let { foo:baz, bar: a='2' } = {foo: 'a'} // baz:2 foo:undefined a:2
let [a, b, c, d ]  = "hello" //a=h b=e
let { length } = "hello" //length=5
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true

let z;
({ z } = { z: 1 })
```