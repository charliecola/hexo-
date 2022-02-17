<!--
 * @Author: 赵金鑫
 * @Date: 2022-02-14 15:24:50
 * @LastEditTime: 2022-02-16 14:17:21
 * @LastEditors: 赵金鑫
 * @Description:
 * @FilePath: \hexo\source\_posts\JS原型与原型链.md
-->

## 普通对象与函数对象

js 中万物皆对象! 但对象也有区别.分为**普通对象和函数对象**,
Object、Function 是 js 自带的函数对象.

```
var o1 = {};
var o2 = new Object();
var o3 = new f1();

function f1(){};
var f2 = function(){};
var f3 = new Function('str','console.log(str)');

console.log(typeof Object); //function
console.log(typeof Function); //function

console.log(typeof f1); //function
console.log(typeof f2); //function
console.log(typeof f3); //function

console.log(typeof o1); //object
console.log(typeof o2); //object
console.log(typeof o3); //object
```

在上面的例子中 o1 o2 o3 为普通对象,f1 f2 f3 为函数对象**凡是通过 new Function()创建的对象都是函数对象,
其他的为普通对象.**

## 构造函数

```
function Person(name,age,job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function() { alert(this.name) }
}
var person1 = new Person('a',28,'aa')
var person2 = new Person('b',28,'bb')
```

person1 person2 都为**实例**。这两个实例都有一个 constructor（构造函数）属性，指向 Person 构造函数。即:

```
console.log(person1.constructor === Person) // true
console.log(person2.constructor === Person) // true
```

两个概念（构造函数，实例）：
person1 和 person2 都是构造函数 Person 的实例
实例的构造函数属性（constructor）指向构造函数。
