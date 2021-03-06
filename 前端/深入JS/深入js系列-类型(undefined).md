##### undefined(未定义)
什么叫未定义？
```js
let a //这是未定义？ 还是说一个未存在的变量b是未定义
```
引入一个C++的概念，声明和定义
```js
let a = 1 //定义：可以为变量分配存储空间，并且可以给变量一个初始值

let b //声明：告诉编译器这个变量的名字和类型
```

##### js无法在程序内安全识别区分未定义变量和未声明变量
```js
// let a 声明
// 未声明
try {
  a
} catch (error) {
  error && console.log("变量未定义");
}
```
##### typeof对未定义变量和未声明变量处理一视同仁

```js
let a = 1
b //没有声明的变量
typeof a //undefined
typeof b  //undefined
```

在这种机制，typeof提供了一种对未定义变量和未声明变量100%安全的检测方法
```js
//DEBUG 是一个开发环境下定义的全局变量，生产环境下没有这个变量
// 这样会抛出错误，ReferenceError: DEBUG is not defined
if (DEBUG) {
console.log( "Debugging is starting" );
}
// 这样是安全的
if (typeof DEBUG !== "undefined") {
console.log( "Debugging is starting" );
}
```

也许你不知道,**undefined并不是一个关键字**，这意味着我们可以**定义**它或者给它**赋值**,但是永远不要这样子做
```js
undefined = 2 //可以,但是没效果

// 可以定义一个变量叫undefined,但是永远不要这样做

function foo() {
"use strict";
var undefined = 2;
console.log( undefined ); // 2
}
foo();
```

#####使用void 0获取安全的undefined

```js
void 0 // undefined
```

#####undefined用途
undefined只是作为系统给未定义变量的初始值，我们最好不要手动把它赋值给变量

#####undefined检测

undefined在规范下，只出现在变量未定义或者未声明情况下，我们使用下面100%安全的检测方法
```js
  typeof v === "undefined" //检测一个变量未定义或未声明
```