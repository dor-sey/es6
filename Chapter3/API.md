# ES5方法
## charAt()
返回指定位置对应的字符,参数是从0开始编号的位置。
* 这个方法可以被数组下标代替

```
var s = 'abc;
s.charAt(1) // "b"
s[1] // "b"
```

## charCodeAt()
获取字符的Unicode编码，不能获取Unicode大于0XFFFF的字符
## String.fromCharCode() 
获取Unicode对应的字符，不能获取Unicode大于0XFFFF的字符
## concat() 
用于连接两个字符串，返回一个新字符串，不改变原字符串。

```
var str = '123'
var newStr = str.concat('456') //123456
//str 123
```

## slice()
用于从原字符串取出子字符串并返回，不改变原字符串。
* 使用范围：Array 和String
* 语法：str.slice(start, end)
* 参数：
    * start: 必需。规定从数组（字符串）的哪个index开始选取，如果是负数，则从数组（字符串）尾部算index，比如，-1指最后一个元素，-2指倒数第二个元素。
    * end:可选。规定到哪个index之前结束。如果没有指定该参数，那么切分的数组（字符串）包含从start到数组（字符串）结束的所有元素。如果这个参数是负数，那么它规定的是从数组（字符串）尾部开始算起的元素。
* 如果参数不是Number类型会先转为Number类型,NaN相当于0

```
var str = '1234567890';
str.slice(0) //1234567890
str.slice(1,5) //2345
str.slice('1','5') //2345
str.slice('asda',5) //12345
str.slice(true, 5) //2345 Number(true):1
str.slice(-1, -1) //''
str.slice(-3,-1) // 89
```

## substring() 
用于从原字符串取出子字符串并返回，不改变原字符串。
* 适用范围： String
* 语法：str.substring(start, end)
* 参数：
    * start: 必需。一个非负的正数，规定要提取的子串的第一个字符在string中的位置。
    * end: 可选。一个非负的正数，规定到哪个index之前结束，如果省略该参数，那么返回的子串会一直到字符串的结尾。

```
var str = '1234567890';
str.substring(0) //1234567890
str.substring(1,5) //2345
str.substring(-3,-1) // ''
```

## substr()
在字符串中抽取从start下标开始的指定长度的字符。
* 语法：str.substr(start, length)
* 参数：
    * start: 必需。要抽取的子串的index。必需是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。
    * 　length: 可选。字串中的字符数，必须是数值，如果省略了该参数，那么返回从字符串的开始位置到结束的字符串

```
var str = '1234567890';
str.substr(0) //1234567890
str.substr(1,5) //23456
str.substr(-3,3) // 890
```

## indexOf() lastIndexOf()
用于确定一个字符串在另一个字符串中的位置，indexOf从字符串头部开始匹配，lastIndexOf从尾部开始匹配
* 参数：字符串，非字符串类型会转为字符串
* 返回值：返回一个整数，表示匹配开始的位置。如果返回-1，就表示不匹配

```
var str = '1230trueNaN123'
str.indexOf('123') //0
str.lastindexOf('123') //11
str.indexOf('1234') // -1
str.indexOf(2) //1
str.indexOf(NaN) //8
str.indexOf(true) //4
```

## trim()
用于去除字符串两端的空格，返回一个新字符串，不改变原字符串。

```
var str = ' 1 2 34 '
str.trim() // '1 2 34'
```

## toLowerCase()，toUpperCase()
toLowerCase方法用于将一个字符串全部转为小写，toUpperCase则是全部转为大写。它们都返回一个新字符串，不改变原字符串。

```
var str ='abSDC'
str.toLowerCase() //absdc
str.toUpperCase() //ABSDC
```
## toLocaleLowerCase()，toLocaleUpperCase()
toLocaleLowerCase方法用本地化的方式将一个字符串全部转为小写，toLocaleUpperCase用本地化的方式转为大写

## localeCompare() 
用本地特定的顺序来比较两个字符串。
* 语法：stringObject.localeCompare(target)
* 返回值：说明比较结果的数字。如果 stringObject 小于 target，则 localeCompare() 返回小于 0 的数。如果 stringObject 大于 target，则该方法返回大于 0 的数。如果两个字符串相等，或根据本地排序规则没有区别，该方法返回 0。
* 说明： 把 < 和 > 运算符应用到字符串时，它们只用字符的 Unicode 编码比较字符串，而不考虑当地的排序规则。以这种方法生成的顺序不一定是正确的。例如，在西班牙语中，其中字符 “ch” 通常作为出现在字母 “c” 和 “d” 之间的字符来排序。
localeCompare() 方法提供的比较字符串的方法，考虑了默认的本地排序规则。ECMAscript 标准并没有规定如何进行本地特定的比较操作，它只规定该函数采用底层操作系统提供的排序规则。

```
var str = 'abc'
var str1 = 'bca'
str.localeCompare(str1) //-1 首先比较第一个字符的Unicode编码，如果相等再往下比，否则结束
```

## split()
用于把一个字符串分割成字符串数组
* 语法：stringObject.split(separator,howmany)
* 参数：
    * separator: 必需。字符串或正则表达式，从该参数指定的地方分割 stringObject。
    * howmany： 可选。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。
* 返回值： 一个字符串数组。该数组是通过在 separator 指定的边界处将字符串 stringObject 分割成子串创建的。返回的数组中的字串不包括 separator 自身。
但是，如果 separator 是包含子表达式的正则表达式，那么返回的数组中包括与这些子表达式匹配的字串（但不包括与整个正则表达式匹配的文本）。

```
var str = "a bc"
str.split("") //["a", "", "b", "c"]
str.split("",3) //["a", "", "b"]
```   

## match()
用于确定原字符串是否匹配某个子字符串，返回一个数组，成员为匹配的第一个字符串。如果没有找到匹配，则返回null。
* 参数：字符串或正则
* 返回值：匹配到元素会返回数组，匹配不到会返回null

```
var str="1 plus 2 equal 31"
str.match('1'); //["1"]
str.match('12')' //null
str.macth(/\d+/g) ['1', '2', '31']
```

## search() 
用法等同于match，但是返回值为匹配的第一个位置。如果没有找到匹配，则返回-1。

```
ar str="1 plus 2 equal 31"
str.match('1'); // 0
str.match('12')' //-1
str.macth(/\d+/g) //0
```

## replace() 
用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有g修饰符的正则表达式）。
* 返回值 ：一个新的字符串，是用 replacement 替换了 regexp 的第一次匹配或所有匹配之后得到的。
* 说明：字符串 stringObject 的 replace() 方法执行的是查找并替换的操作。它将在 stringObject 中查找与 regexp 相匹配的子字符串，然后用 replacement 来替换这些子串。如果 regexp 具有全局标志 g，那么 replace() 方法将替换所有匹配的子串。否则，它只替换第一个匹配子串。
replacement 可以是字符串，也可以是函数。如果它是字符串，那么每个匹配都将由字符串替换。但是 replacement 中的 $ 字符具有特定的含义。
    * \$1、\$2、...、\$99: 与 regexp 中的第 1 到第 99 个子表达式相匹配的文本。
    * $&：与 regexp 相匹配的子串。
    * $`：位于匹配子串左侧的文本。
    * $'：位于匹配子串右侧的文本。
    * $$：直接量符号

```
var str="Visit Microsoft!"
str.replace("Microsoft", "") //Visit !;

//把 "Doe, John" 转换为 "John Doe" 的形式：
name = "Doe, John";
name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1");

//把所有的花引号替换为直引号
name = '"a", "b"';
name.replace(/"([^"]*)"/g, "'$1'");

//把字符串中所有单词的首字母都转换为大写
name = 'aaa bbb ccc';
uw=name.replace(/\b\w+\b/g, function(word){
  return word.substring(0,1).toUpperCase()+word.substring(1);}
  );
```

## valueOf()
返回字符串对象的原始值

# ES6方法
## codePointAt() 
获取字符的Unicode编码

```
let s = '𠮷a';

s.codePointAt(0).toString(16) // "20bb7"
s.codePointAt(2).toString(16) // "61"
```

## String.fromCodePoint()
获取Unicode对应的字符

```
String.fromCodePoint(0x20BB7)
// "𠮷"
```

##  normalize()
将字符的不同表示方法统一为同样的形式，称为 Unicode 正规化。
normalize方法可以接受一个参数来指定normalize的方式，参数的四个可选值如下。
* NFC，默认参数，表示“标准等价合成”，返回多个简单字符的合成字符。所谓“标准等价”指的是视觉和语义上的等价。
* NFD，表示“标准等价分解”，即在标准等价的前提下，返回合成字符分解的多个简单字符。
* NFKC，表示“兼容等价合成”，返回合成字符。所谓“兼容等价”指的是语义上存在等价，但视觉上不等价，比如“囍”和“喜喜”。（这只是用来举例，normalize方法不能识别中文。）
* NFKD，表示“兼容等价分解”，即在兼容等价的前提下，返回合成字符分解的多个简单字符。

```
'\u004F\u030C'.normalize('NFC').length // 1
'\u004F\u030C'.normalize('NFD').length // 2
```

## includes(), startsWith(), endsWith()

includes()：返回布尔值，表示是否找到了参数字符串。 第二个参数表示从哪开始找
startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。第二个参数表示从哪开始找
endsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。第二个参数表示前n个字符

```
let s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

## repeat() 
repeat方法返回一个新字符串，表示将原字符串重复n次
* 参数如果是小数，会被取整
* 如果取整后的参数是负数或者Infinity，会报错
* 参数NaN等同于0
* 如果repeat的参数是字符串，则会先转换成数字。

```
'x'.repeat(3) // "xxx"
'na'.repeat(2.9) // "nana"
'na'.repeat(Infinity)
// RangeError
'na'.repeat(-1)
// RangeError
'na'.repeat(-0.9) // "" 取整为-0 相当于0 
'na'.repeat('na') // "" 转成数字为NaN 相当于0
'na'.repeat('3') // "nanana"
```

## padStart()，padEnd()
字符串补全长度，padStart()用于头部补全，padEnd()用于尾部补全。
padStart()和padEnd()一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。
* 如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。
* adStart()和padEnd()一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。
* 如果省略第二个参数，默认使用空格补全长度。

```
'x'.padStart(5, 'ab') // 'ababx'
'xxx'.padStart(2, 'ab') // 'xxx'
'abc'.padStart(10, '0123456789')
'x'.padStart(4) // '   x'
```

## String.raw()
用来充当模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，对应于替换变量后的模板字符串。

```
String.raw`Hi\n${2+3}!`;
// 返回 "Hi\\n5!"
```

* String.raw方法可以作为处理模板字符串的基本方法，它会将所有变量替换，而且对斜杠进行转义，方便下一步作为字符串来使用。
* String.raw方法也可以作为正常的函数使用。这时，它的第一个参数，应该是一个具有raw属性的对象，且raw属性的值应该是一个数组。

```
String.raw({ raw: 'test' }, 0, 1, 2);
// 't0e1s2t'

// 等同于
String.raw({ raw: ['t','e','s','t'] }, 0, 1, 2);
```

## for..of循环
ES6 为字符串添加了遍历器接口（详见《Iterator》一章），使得字符串可以被for...of循环遍历。
```
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```

