###javascript运行环境
js如果只在引擎中运行，它会严格遵循并且可以预测的，但是js几乎都在宿主环境中运行，**浏览器或者Node环境**

#### ECMAScript中的Annex B
介绍了浏览器兼容性问题导致与官方规范的差异

1. 在非严格模式中允许八进制数值常量存在，如0123（即十进制的83）。

2. window.escape(..) 和window.unescape(..) 让你能够转义（escape）和回转（unescape）带有% 分隔符的十六进制字符串。例如，window.escape( "? foo=97%&bar=3%" ) 的结果为"%3Ffoo%3D97%25%26bar%3D3%25"。

3. String.prototype.substr 和String.prototype.substring 十分相似，除了前者的第二个参数是结束位置索引（非自包含），后者的第二个参数是长度（需要包含的字符数）。

#### Web ECMAScript规范，介绍官方ECMAScript和目前浏览器中JavaScript实现的差异

<!-- 和--> 是合法的单行注释分隔符。

1. String.prototype 中返回HTML 格式字符串的附加方法：anchor(..)、big(..)、
blink(..)、bold(..)、fixed(..)、fontcolor(..)、fontsize(..)、italics(..)、
link(..)、small(..)、strike(..) 和sub(..)**以上内容在实际开发中很少使用，也不推荐，我们更倾向于使用其他的内建**

2. DOM API 和自定义工具集。

3. RegExp 扩展：RegExp.$1 .. RegExp.$9（匹配组） 和RegExp.lastMatch/RegExp["$&"]（最近匹配）。

4. Function.prototype 附加方法：Function.prototype.arguments（别名为arguments 对象）和Function.caller（别名为arguments.caller）。

#### 宿主对象 由宿主对象创建并提供给js引擎的变量，包括内建对象和函数
常见的宿主对象，如下例子
```js
var a = document.createElement( "div" );
typeof a; // "object"--正如所料
Object.prototype.toString.call( a ); // "[object HTMLDivElement]"
a.tagName; // "DIV"
```
需要注意**宿主对象的行为差异**

1. 无法访问正常的object 内建方法，如toString()

2. 无法写覆盖

3. 包含一些预定义的只读属性

4. 包含无法将this 重载为其他对象的方法

#### 全局DOM变量

一个不太为人所知的事实是：由于浏览器演进的历史遗留问题，**在创建带有id 属性
的DOM 元素时也会创建同名的全局变量**
```js
<div id="foo"></div>
  if (typeof foo == "undefined") {
    oo = 42; // 永远也不会运行
  }
console.log( foo ); // HTML元素
```
当页面出现多个id同名时，我们可以直接使用该全局变量来获取同id的HTMLCollection

故意设置两个id同名，当然编码时不推荐，简易保持唯一
![](https://img2018.cnblogs.com/blog/1361028/201903/1361028-20190321005447181-2011121765.png)

使用id作为变量名来获取同名DOM
![](https://img2018.cnblogs.com/blog/1361028/201903/1361028-20190321005517270-736407046.png)

#### 原生原型
**不要扩展原生原型，因为随着规范发展，扩展的功能可能会被实现，使得扩展和规范冲突**

必要的扩展建议作如下处理
```js
if (!Array.prototype.push) {
// Netscape 4没有Array.push
  Array.prototype.push = function(item){
    this[this.length-1] = item;
  };
}
```

##### shim/polyfill

polyfill 能有效地为不符合最新规范的老版本浏览器填补缺失的功能，让你能够通过可靠的代码来支持所有你想要支持的运行环境,shim/polyfill能够填充新的API，但是没法填充新的语法，我们可以使用bable来将新的语法转换成旧的语法



##### 宿主环境实现的限制

下面列出一些已知的限制：

1. 字符串常量中允许的最大字符数（并非只是针对字符串值）

2. 可以作为参数传递到函数中的数据大小（也称为栈大小，以字节为单位）

3. 函数声明中的参数个数

4. 未经优化的调用栈（例如递归）的最大层数，即函数调用链的最大长度

5. JavaScript 程序以阻塞方式在浏览器中运行的最长时间（秒）

6. 变量名的最大长度