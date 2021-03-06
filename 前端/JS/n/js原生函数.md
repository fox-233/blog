String()
Number()
Boolean()
Array()
Object()
Function()
RegExp()
Date()
Error()
Symbol()——ES6 中新加入的！

内部属性[[Class]]
我们可以通过Object.prototype.toString.call()来查看
```js
Object.prototype.toString.call( null );
// "[object Null]"
Object.prototype.toString.call( undefined );
// "[object Undefined]"
Object.prototype.toString.call( "abc" );
// "[object String]"
Object.prototype.toString.call( 42 );
// "[object Number]"
Object.prototype.toString.call( true );
// "[object Boolean]"
Object.prototype.toString.call( [1,2,3] );
// "[object Array]"
Object.prototype.toString.call( /regex-literal/i );
// "[object RegExp]"
```
封装对象包装
封装对象（object wrapper） 扮演着十分重要的角色。由于基本类型值没有.length
和.toString() 这样的属性和方法，需要通过封装对象才能访问，此时JavaScript 会自动为
基本类型值包装（box 或者wrap）一个封装对象：
```js
var a = "abc";
a.length; // 3
a.toUpperCase(); // "ABC"
```
一般情况，包装和拆箱都由引擎来决定，过早优化反而会降低引擎的执行效率

拆封 得到封装对象的基本类型值 可以使用valueOf函数
```js
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );
a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true
```

原生函数作为构造函数,推荐使用对象字面值

有以下漏洞
```js
let arr = Array(n) //n会作为长度而不是一个元素
// 而且显示的时候是undefined,但是实际上不是undefined,如果api是根据元素
// 内置api对空元素处理行为不一致

// 生成一个元素按照数组长度递增的数组
let arr = Array(5)
let r = arr.map((e, index) => index+1 )
console.log(arr, r); //empty X 5而不是[1, 2, 3, 4, 5]
```
空元素数组的显示在浏览器和内置api对它们的处理不一致，避免创建和使用空数组

如果需要指定一个数组的长度且用undefined填充
```js
var a = Array.apply(null, {length: 3})
a //[undefined, undefined, undefined]
```
尽量避免Object(..)、Function(..) 和RegExp(..)的使用，
Object 可以用对象字面值形式代替来创建，还可以一次定义多个参数
Function 在动态定义函数参数以及函数体时会用到，一般我们推荐使用function 语法来定义

RegExp 在定义动态表达式会用到

Date() Error()
