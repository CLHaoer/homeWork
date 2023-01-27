## 继承
### 继承的类别
1. 原型链继承     
2. 借用构造函数（经典继承）
3. 组合继承
4. 原型式继承
5. 寄生式继承
6. 寄生式组合继承  ===>最完美的
7.  extends 继承   （===> 语法糖，寄生式组合继承）
### 原型链继承
> 其基本思想就是通过原型继承多个引用类型的属性和方法。

原型链继承。====> 原型链有关

继承：子类拥有父类的属性和方法

类： JS ==> 构造函数 
```js
//父类
function Parent(name){
this.name = name || '父类' //相当于给默认值
this.arr = [1,2,3]
}
Parent.prototype.sayHi = function(){
    console.log('hi~~' )
}

//子类
function Child(hobby){
    this.hobby = hobby
}
//核心 ==> 让子类的原型 等于 父类构造函数的实例
Child.prototype = new Parent() // 相当于child和Parent两个构造函数之间有了关联
Child.prototype.constructor = Child  //手动的修正Child的constructor
const boyl = new Child('sing')
const boy2 = new Child('dance')
console.log(boy1, boy2)
```
- 优点：方法复用，共享，因为方法定义在父类的原型上，实例共享了父类原型上的方法
- 缺点：
  -  子类在实例化的时候，不能给父类构造函数传参
  - 子类的实例共享了父类构造函数的属性和方法如果父类里的属性值是引用类型(数组，对象等)，实例修改这个值后会相互影响