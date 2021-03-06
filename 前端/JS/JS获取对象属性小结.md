最近面试遇到问如何获取对象全部属性名的方法，总结一下：

对象属性类型分类：

1.ESMAScript分类

    数据类型 又分为可枚举和不可枚举类型
    访问器类型 
2.上下文分类

    原型属性
    实例属性 

1.列举自身但不包括原型的可枚举属性名  Object.keys(obj)
```js
  // 遍历对象
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.demo = function() {};

let cj = new Person('cj', 25);
// 通过Object.defineProperty定义一个不可枚举属性
Object.defineProperty(cj, 'weight', {
  enumerable:false 
})

// enumerable = true
// console.log(Object.keys(cj)) // name age

// enumerable = false
// console.log(Object.keys(cj)) // name age weight
```  

2.列举包括自身不可枚举但不包括原型的属性名 Object.getOwnPropertyNames(obj)
```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}
// 设置原型属性
Person.prototype.demo = function() {};

let cj = new Person('cj', 25);
// 通过Object.defineProperty定义一个不可枚举属性
Object.defineProperty(cj, 'weight', {
  enumerable:false 
})
// 获取属性名
console.log(Object.getOwnPropertyNames(cj)) // name age weight
```
3.获取自身和原型链上的可枚举属性 for in  返回的顺序可能与定义顺序不一致
```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}
// 设置原型属性
Person.prototype.demo = function() {};
Object.prototype.j = 1
let cj = new Person('cj', 25);
// 通过Object.defineProperty定义一个不可枚举属性
Object.defineProperty(cj, 'weight', {
  enumerable:false 
})

let props = []
for(prop in cj){
  props.push(prop)
}

console.log(props) //name age weight j
```
4.获取自身Symbol属性 Object.getOwnPropertySymbols(obj)
```js
let obj = {};
// 为对象本身添加Symbol属性名
let a = Symbol("a");
obj[a] = "localSymbol";
// 为对象原型添加Symbol属性名
let b = Symbol("b")
Object.prototype[b] = 111
let objectSymbols = Object.getOwnPropertySymbols(obj);
console.log(objectSymbols); //Symbol(a)
```
5.获取自身包括不可枚举和Symbol属性名，但不包括原型 Reflect.ownKeys(obj)
```js
  // 遍历对象
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.demo = function() {};
let s = Symbol('s')
let cj = new Person('cj', 25);
// 通过Object.defineProperty定义一个不可枚举属性
Object.defineProperty(cj, 'weight', {
  enumerable: false 
})
cj[s] = 1
let a = Symbol('a')
Object.prototype[a] = 1
console.log(Object.getOwnPropertyNames(cj)) //name age weight 
console.log(Reflect.ownKeys(cj)) //name age weight Symbol(s)
```