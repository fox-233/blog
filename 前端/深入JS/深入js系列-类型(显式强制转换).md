###什么是显式
这里的显式和隐式是以普遍的标准来进行讨论的，你能看出来是怎么回事，那么它对你是“显式”，相反你不知道的话，对你就是“隐式”

####抽象操作 
字符串、数字、布尔值之间类型转换的基本规则 ，ES5定义了一些操作，诸如ToString、ToNumber、ToBoolean、ToPrimitive抽象操作

##### ToString 负责处理非字符到字符串的强制类型转换

undefined => "undefined"
null => "null"
number => "number"  极大数和极小数用指数表示
boolean => "boolean"
Symbol => "symbol"
Object => instance.toString ||Object.prototype.toString //规则由抽象操作ToPrimitive抽象操作里完成  

##### ToNumber 非数字到数字的强制类型转换

undefined => NaN
null => 0
boolean => true 1 false 0
Symbol => 无法转换，本身用途是为了解决命名冲突
string => 数字常量规则，失败为NaN，且0开头的十六进制按照十进制处理
Object => 由抽象操作ToPrimitive抽象操作，检查该值是否有valueof方法并且返回基本类型，有就对返回值进行强制转换，没有就使用toString的返回值来强制转换，两者都没有得到基本值，会产生TypeError错误，更详细的转换规则会在ToPrimitive抽象操作详解

##### ToBoolean

###### 假值(falsy value)列表

undefined
null
false
+0、-0、NaN
""

**假值对象 document.all**

假值对象是游览器废弃对象之后，为了分辨版本将其修改为假值对象，document.all就是其中一个例子
![](https://img2018.cnblogs.com/blog/1361028/201903/1361028-20190320100042359-1445921439.png)

那为什么它要是假值呢？因为我们经常通过将document.all 强制类型转换为布尔值（比如
在if 语句中）来判断浏览器是否是老版本的IE。IE 自诞生之日起就始终遵循浏览器标准，
较其他浏览器更为有力地推动了Web 的发展。
if(document.all) { /* it’s IE */ } 依然存在于许多程序中，也许会一直存在下去，这对
IE 的用户体验来说不是一件好事。
虽然我们无法彻底摆脱document.all，但为了让新版本更符合标准，IE 并不打算继续支持
if (document.all) { .. }。
“那我们应该怎么办？”
“也许可以修改JavaScript 的类型机制，将document.all 作为假值来处理！”

###### 真值 除假值列表之外的值

#### 显式强制类型转换

##### 字符串和数字之间的显式转换
```js
var a = 42;
var b = String( a );

var c = "3.14";
var d = Number( c );
b; // "42"
d; // 3.14

var a = 42;
var b = a.toString(); //直接调用toString

var c = "3.14";
var d = +c; // +作为一元运算符来将操作数强制转换为数字
b; // "42"
d; // 3.14
```
#####  显式解析数字字符串 这里要注意的是，解析和转换还是有明显区别,转换不允许出现非数字字符串，看下面例子
```js
var a = "42";
var b = "42px";
Number( a ); // 42
parseInt( a ); // 42
Number( b ); // NaN
parseInt( b ); // 42
```
     
##### 显式转换为布尔值 非布尔值转换到布尔值的情况
```js
// 使用Boolean(),不过不常用
var a = "0";
Boolean( a ); // true
var b = [];
Boolean( b ); // true
var c = {};
Boolean( c ); // true
var d = "";
Boolean( d ); // false
var e = 0;
Boolean( e ); // false
var f = null;
Boolean( f ); // false
var g;
Boolean( g ); // false

//更加常用的方式 !!
!!a // boolean,里面的!将非布尔值转换到布尔值并取反，加一个!对取反的结果再次取反，得到原本的布尔值
```