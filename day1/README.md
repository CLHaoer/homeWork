## 作用域
- 全局作用域
- 局部作用域（函数作用域）
- 块级作用域
### 全局作用域
- 整个document文档就相当于一个全局作用域，通过`<script>`便签和 .js文件中声明的全局变量，在一个文档中都能访问到。

```html
<script>
    let a = 1 
</script>
<script src='a.js'></script>
```
### 局部作用域
> 局部作用域: 函数作用域，块级作用域
- 函数作用域：在函数内部声明的变量，在函数外部不能直接访问。函数执行完毕后，会清除变量
```js
function fn(){
    //函数内部声明，局部变量
    let a = 1 
    const b = 2
    console.log(a,b)// 1,2 函数体内可以访问
}
console.log(a,b)// a,b is not undefined 外部不能直接访问
```
- 块级作用域：用大括号`{}`包裹的，且是let,const关键字声明的变量才能产生块级作用域。

```js
{
    let a = '这是一个款及作用域'
}
for(let i = 0 ,i < 10 , i++){
    console.log(i)//这里的i也是一个块级作用域
}
```
注：1.var关键字没有块级作用域
2.不同代码块之间的变量不能相互访问

### 作用域链
- 作用域链就是使用变量时，变量的的查找机制
```js
let a = 1
let b = 2 //全局作用域
function fn(){
    let a = 11 //局部作用域
    function gn(){
        let b = 22 //局部作用域
        console.log(a,b) //11,22
    }
    gn()
    console.log(a,b)//11,2
}
fn()
console.log(a,b)//1,2
```
作用域链的查找就是从当前作用域向全局作用域的方向，一层一层的查找，找到之后就停止查找，开始引用。

注：作用域链只能向上层查找，不能向下查找

## 垃圾回收机制
- 垃圾回收（英语：Garbage Collection，缩写为GC）是指一种自动的内存管理机制。当某个程序占用的一部分内存空间不再被这个程序访问时，这个程序会借助垃圾回收算法向操作系统归还这部分内存空间。
### 内存生命周期
不管什么程序语言，内存生命周期基本是一致的：

1. 分配你所需要的内存
2. 使用分配到的内存（读、写）
3. 不需要时将其释放\归还

所有语言第二部分都是明确的。第一和第三部分在底层语言中是明确的，但在像 JavaScript 这些高级语言中，大部分都是隐含的。
```js
var n = 123; // 给数值变量分配内存
var s = "azerty"; // 给字符串分配内存

var o = {
  a: 1,
  b: null
}; // 给对象及其包含的值分配内存

// 给数组及其包含的值分配内存（就像对象一样）
var a = [1, null, "abra"];

function f(a){
  return a + 2;
} // 给函数（可调用的对象）分配内存

// 函数表达式也能分配一个对象
someElement.addEventListener('click', function(){
  someElement.style.backgroundColor = 'blue';
}, false);
```
### 垃圾回收策略
- 引用计数法
>目前主要是IE8 以下的浏览器使用，现代浏览器都弃用了这种方式。基本原理：记录跟踪每个值被引用的次数，被引用一次，次数就加一；被释放就减一；为零时就释放当前值所占内存。
```js
var o = {
  a: {
    b:2
  }
};
// 两个对象被创建，一个作为另一个的属性被引用，另一个被分配给变量 o
// 很显然，没有一个可以被垃圾收集

var o2 = o; // o2 变量是第二个对“这个对象”的引用
o = 1;      // 现在，“这个对象”只有一个 o2 变量的引用了，“这个对象”的原始引用 o 已经没有
var oa = o2.a; // 引用“这个对象”的 a 属性
               // 现在，“这个对象”有两个引用了，一个是 o2，一个是 oa
o2 = "yo"; // 虽然最初的对象现在已经是零引用了，可以被垃圾回收了
           // 但是它的属性 a 的对象还在被 oa 引用，所以还不能回收
oa = null; // a 属性的那个对象现在也是零引用了
           // 它可以被垃圾回收了
```
缺点：变量被循环引用的时候，就无法清除
```js
function f(){
  var o = {};
  var o2 = {};
  o.a = o2; // o 引用 o2
  o2.a = o; // o2 引用 o

  return "azerty";
}

f();
```
- 标记清除法
> 当变量进入执行环境时标记为“进入环境”，当变量离开执行环境时则标记为“离开环境”，被标记为“进入环境”的变量是不能被回收的，因为它们正在被使用，而标记为“离开环境”的变量则可以被回收
```js
function func3 () {
      const a = 1
      const b = 2
      // 函数执行时，a b 分别被标记 进入环境
}

func3() // 函数执行结束，a b 被标记 离开环境，被回收
```
## 闭包
> 闭包 = 内层函数 + 外层函数的变量
- 理解：内层函数引用外层函数得变量。
- 作用：外部作用域可以访问函数内部得变量。实现了数据得私有化。
- 缺点：会造程内存泄漏的问题。
```js
function aa(){
    let i = 0
    return function(){
        i++
        console.log(i)
    }
}
let bb = aa()
bb()
//aa()返回值是一个函数，并赋值给bb，此时bb作为全局变量所使用。
//所以此时bb()被调用就会执行函数内部的函数，进行计算，从而造成内存泄漏。知道页面被关闭时才会整体清除。
```
闭包的应用：防抖，节流

## 变量提升
### var提升
- var定义的变量会提升到当前作用域最前面，提升的只是变量的声明。
```js
function fn(){
    console.log(a)//undefined
    var a = 1       
    console.log(a)//1
}
//相当于
function fn(){
    var a
    console.log(a)
    a = 1
    console.log(a)
}
```
### 函数提升
- 用function声明的函数会函数提升，是将整个函数进行提升。
```js
fn()
function fn(){
    console.log('整个部分全部提升')
}
//相当于
var fn
fn = function (){
    console.log('整个部分全部提升')
}
//这里可以看作是var，但是函数是声明和赋值是一起的
fn()
```
## 函数参数
### 动态参数
- arguments 动态参数 只存在于 函数里面
- 是伪数组 里面存储的是传递过来的实参
```js
function fn(){
    console.log(arguments)//[1,2,3]
}
fn(1,2,3)
//不传参数的时候，arguments为null
```
### 剩余参数
- 剩余参数`...`将函数中多余没被使用的参数封装成一个真的数组。
```js
function getSum(...arr){
  console.log(arr)// [2,3,4,1,3]
}
getSum(2,3,4,1,3)

function getSum1(a,b,...arr1){
  console.log(arr1)// [45,6,3,2]
}
getSum1(5,2,45,6,3,2)
```
### 扩展运算符
- `...`就是扩展运算符
```js
//'...'扩展运算符
const arr = [3,4,2,4,5,6]
console.log(...arr)            //3 4 2 4 5 6
console.log(Math.max(...arr))  //6

//扩展运算符可以进行 浅拷贝
const a = {
    name:'ttt'
}
const b = {...a}
b.name = 'aaa'
console.log(a.name) // ttt

//扩展运算还可以用于合并数组
const a1 = [3,4,54,5,6]
const a2 = [7,8,9,1]
const newarr = [...a1,...a2] // [3,4,54,5,6,7,8,9,1]
```

```js
//数组中合并数组的方法 concat()
const a1 = [3,4,54,5,6]
const a2 = [7,8,9,1]
const newarr = a1.concat(a2)// [3,4,54,5,6,7,8,9,1]
```
## 箭头函数   () =>

```js
//定义一个箭头函数
let a = () => {/*code*/}
//当形参是由一个时，可以省略小括号
let b = x => {/*code*/}
//当后续只有一行代码时，可以省略 大括号 ，此用法相当于返回 数据，只不过return被省略了
let c = y => (return) /*code*/
//当返回值为对象 ，大括号冲突了，用小括号包括
let d = z => ({/*code*/})
```

- 箭头函数 `this` 指向问题：
  - ⭐⭐箭头函数本身是没有`this`，它的this指向的是上层作用域中的this⭐⭐
  - 作用域：全局作用域，函数作用域，块级作用域
- 箭头函数没有arguments属性。
```js
const fn = () => {
  console.log(this)  // window
}
fn()
// 对象方法箭头函数 this
const obj = {
  uname: 'pink老师',
  sayHi: () => {
    console.log(this)  // this 指向window
  }
}
obj.sayHi()
```