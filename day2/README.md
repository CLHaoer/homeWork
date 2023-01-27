## 结构赋值
> 解构赋值是一种快速为变量赋值的简洁语法，本质上仍然是为变量赋值。
### 数组的解构
> 数组解构是将数组的单元值快速批量赋值给一系列简洁写法。

基本语法：

- 赋值运算符 = 左侧的 [] 用于批量声明变量，右侧数组的单元值将被赋值给左侧的变量
- 变量的顺序对应数组单元值的位置依次进行赋值操作

```js
// 1. 变量多， 单元值少 ， undefined
const [a, b, c, d] = [1, 2, 3]
console.log(a) // 1
console.log(b) // 2
console.log(c) // 3
console.log(d) // undefined
// 2. 变量少， 单元值多
const [a, b] = [1, 2, 3]
console.log(a) // 1
console.log(b) // 2
// 3.  剩余参数 变量少， 单元值多
const [a, b, ...c] = [1, 2, 3, 4]
console.log(a) // 1
console.log(b) // 2
console.log(c) // [3, 4]  真数组

// 4. 解构赋值允许指定默认值 
const [a = 0, b = 0] = [1, 2]
console.log(a)
console.log(b)
const [a = 0, b = 0] = []  
const [a = 0, b = 1] = ['undefined', undefined]
console.log(a) // undefined string
console.log(b) // 1
// 5.  按照对应位置，对变量赋值。
const [a, b, , d] = [1, 2, 3, 4]
console.log(a) // 1
console.log(b) // 2
console.log(d) // 4

// 多维数组: 数组嵌套数组
const arr = [1, 2, [3, 4]]
console.log(arr[0])  // 1
console.log(arr[1])  // 2
console.log(arr[2])  // [3,4]
console.log(arr[2][0])  // 3

// 多维数组解构
const arr = [1, 2, [3, 4]]
const [a, b, c] = [1, 2, [3, 4]]
console.log(a) // 1
console.log(b) // 2
console.log(c) // [3,4] 

const [a, b, [c, d]] = [1, 2, [3, 4]]
console.log(a) // 1
console.log(b) // 2
console.log(c) // 3
console.log(d) // 4

```

**注**：

对于数组的解构赋值，是严格按照顺序进行解构赋值的。

### 对象的解构

基本语法：

- 赋值运算符 = 左侧的 {} 用于批量声明变量，右侧对象的属性值将被赋值给左侧的变量
- 对象属性的值将被赋值给与属性名相同的变量
- 注意解构的变量名不要和外面的变量名冲突否则报错
- 对象中找不到与变量名一致的属性时变量值为undefined

给新的变量名重新命名：

```js
const obj = {
  name:'张三',
  age:18
}
const {name:sb , age} = obj //name为对象的属性，:之后为变量名
console.log(sb,age)//张三 18
```

多级对象解构:

```js
let options = {
  size: {
    width: 100,
    height: 200
  },
  items: ["Cake", "Donut"],
  extra: true
};

// 为了清晰起见，解构赋值语句被写成多行的形式
let {
  size: { // 把 size 赋值到这里
    width,
    height
  },
  items: [item1, item2], // 把 items 赋值到这里
  title = "Menu" // 在对象中不存在（使用默认值）
} = options;

console.log(title);  // Menu
console.log(width);  // 100
console.log(height); // 200
console.log(item1);  // Cake
console.log(item2);  // Donut
```

## Array.filter 方法

> **`filter()`** 方法创建给定数组一部分的[浅拷贝](https://developer.mozilla.org/zh-CN/docs/Glossary/Shallow_copy)，其包含通过所提供函数实现的测试的所有元素。

```js
const arr = [1,23,34,5,46,3,78]
const newArr = arr.filter(el=>{
  return el>30
})
console.log(newArr) //[34,46,78]
//用法：
被遍历的数组.filter(function(当前值,索引,原素组){
   return 条件
})
```

**注**：

Array.filter()方法返回满足条件的一个新数组，没有满足条件时就返回一个空数组。

## 构造函数

> 通过构造函数的new操作符创建一个新的对象

- 对象中的变量称为 属性，对象中的函数称为 方法
- 属性：事物的特征（常用名词），方法：事物的行为（常用动词）

构造函数在技术上是常规函数。不过有两个约定：

1. 它们的命名以大写字母开头。
2. 它们只能由 `"new"` 操作符来执行。

```js
function Person(name,age){
  this.name = name
  this.age = age
}
const p = new Person('张三',18)
console.log(p)//{name:'张三',age:18}
```

new操作符执行过程：

当一个函数被使用 `new` 操作符执行时，它按照以下步骤：

1. 一个新的空对象被创建并分配给 `this`。
2. 函数体执行。通常它会修改 `this`，为其添加新的属性。
3. 返回 `this` 的值。

```js
//用that代替this
function Person(name,age){
  const that = {}
  that.name = name
  that.age = age
  return that
}
```

**手写一个new：**

1. 创建一个空的简单JavaScript对象（即{}）
2. 为步骤1新创建的对象添加`__proto__`，将该属性链接至构造函数的原型对象
3. 将步骤1新创建的对象作为this的上下文
4. 如果该函数没有返回值，则返回this

> new关键词执行后总会返回一个对对象，要么是实例对象，要么是return语句指定的对象

```js
function _new(fn,...args){
  //let obj = new Object()
  //obj.__proto__ = fn.prototype
  //基于fn构造函数原型创建一个新对象
  let obj = Object.create(fn.prototype)
  //执行构造函数，并获取fn执行的结果
  let res = fn.call(obj,...args)
  //如果执行结果有返回值并且是一个对象，返回执行结果，否则返回新创建的对象
  let isObject = typeof res === 'object' && res !== null
  let isFunction = typeof res === 'function'
  return isObject || isFunction ? res : obj
}
```

**实例成员/静态成员**：

- 实例成员就是构造函数内部通过this添加的成员
-  实例成员通过实例化的对象来访问

```js
function Person(name,age){
  this.name = name
  this.age = age
}
const p = new Person('张三',18)
console.log(p.name)//实例成员
```

- 静态成员,  在构造函数本身上添加的成员
- 静态成员通过构造函数来访问

```js
function Person(name,age){
  this.name = name
  this.age = age
}
Person.sayHello = function (){
  console.log('Hello World')
}
```

注：

- 一般公共特征的属性或方法设置成静态成员
- 静态成员方法中的this指向构造函数本身
- 由构造函数创建的实例对象彼此独立，互不影响
