



# 基础

## 数据类型

### 对象类型本质只有一类：

​	Object：任意对象，除了基本类型万物都是它
​	Function：特别的对象（可以执行）
​	Array：特别的对象，又下标，有序

### 判断：

typeof: 返回数据类型的字符串表达, 小写
	可以判断undefined/function/ 数值、字符串、布尔
	不能判断null object array

intanceof：判断实例的类型 ([] instanceof Array)
===/==

判断是不是undefined：

1. type of a === 'undefined'
2. a===undefined

判断null：a === null

### null和undefined

区别：undefined表示定义没有赋值，null表示赋值了只是值是null

使用null场景：

+ 初始赋值，表明将来是个对象
+ 结束前，释放内存，触发垃圾回收

## 函数

### 调用方法

+ func ( ) 
+ obj.func( )
+ new test ( ) 
+ test.call/apply(obj) 临时让test成为obj的方法进行调用

### 回调函数

你定义的，你没有用，最终某处会用
有dom事件回调函数、定时器回调函数、ajax回调函数，生命周期回调函数

### IIFE立即执行函数表达式

```javascript
!(function () { // 匿名函数自我调用
  // IIFE
  // 隐藏实现
  // 不会污染外部全局命名空间
})()
```

### this

是什么：

+ 任何函数本质上都是通过某个变量调用的，如果没有就直接指定是window
+ 所有函数内部都有一个变量this
+ this是调用函数的当前对象

如何确定：

+ test( ) window
+ p.test( ) p
+ new test( ) 新建的对象
+ test.call (obj): obj

# 原型

## 原型prototype

### 函数的prototype属性

+ 每个函数都有一个**prototype属性**，它默认指向一个Object对象，称为**原型对象**
+ 原型对象中有一个**constructor属性**，指向函数对象，对没错，又指回来了
+ 原型对象和函数对象两个对象可以互相引用 func.prototype.constructor === func

### 给原型对象添加属性（一般都是添加一个方法）

作用：函数所有的实例对象自动拥有了原型中添加的属性（方法），比如vue中Vue.prototype.$axios = axios



## 显示原型和隐式原型

![image-20210404204315957](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210404204315957.png)

1. 每个函数function都有一个prototype属性，称为显示原型（可以看做定义构造函数的时候，this.prototype = { }）
2. 每个实例对象都有一个proto，称为隐式原型
3. 对象的隐式原型的值为其对应的构造函数的显示原型的值（可以看成当创建实例的时候`this.__proto__ = Fn.prototype`）
4. prototype属性是定义函数的时候自动添加的，默认值是个空Object对象
5. `__proto__`属性是在创建对象实例的时候添加的，默认是构造函数的prototype属性
6. 程序员直接操作显示原型，但是不应该直接操作隐式模型



## 原型链

![image-20210404220142620](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210404220142620.png)

1. 访问一个对象属性的时候，现在自身属性中查找，找到了就返回
2. 如果没有，沿着`__proto__`链向上查找，找到后返回
3. 最后都没找到返回undefined
4. 别名：**隐式原型链**（在原型链寻找的时候根本没管显示原型）
5. 原型链作用：查找对象的属性和方法



一下简称`__proto__` 为proto

首先来明确三条规律：

1. 函数对象都有一个原型对象(看成一个空的Object对象)
2. 所有函数都是Function的实例，包括他自己也是自己的实例,因此所有函数的proto都指向了 Function prototype
3. 所有的原型**对象**都是Object的实例, (Object自己的原型对象例外), 因此所有的原型对象的proto都指向了Object prototype

好了, 现在可以来提炼线条了:

![image-20210408112726486](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210408112726486.png)

1. 首先要明确, 你自己定义了一个Foo函数对象, JavaScript定义了Object函数对象和Function函数对象, 这三个在图中是直角正方形
2. 由第一条规则, 每个函数对象都有一个他的原型对象, 所以你注意到图中所有的函数的原型都指向了含有prototype的圆角矩形
3. 由第二条规律, 上图中, 所有的函数对象, 就是尾巴有括号的这些方块, 看他们的proto, 全部都指向了Function prototype, 这些都是原型对象
4. 所有的圆角矩形就是都还是一个**对象**, 因此它们还是Object的实例, 他们的proto都指向了Object的原型对象
5. Object的原型对象就是原型链尽头, 他比较特殊, 他的proto指向了null



最后我们就可以总结原型链了:

1. Foo > Function prototype > Object prototyoe > null
2. foo = new Foo( ) > Foo prototype > Object Prototype > null
3. o = new Object( ) > Object prototype > null



练习题:

![image-20210406101244287](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210406101244287.png)

原型链: 
f > F prototype > Object Prototype > null
F > Function prototype > Object prototype > null
因此f不能执行b, F都可以执行

# 闭包

闭包就是能够读取其他函数内部变量的函数
还有种理解是，闭包是内部函数的一个对象，保存了内部函数对外部函数的变量的引用
总之，被内部函数引用了的变量是可达的，会常驻内存

## 使用场景

1. 函数作为另一个函数的返回值
2. 函数作为参数传递给另一个函数

## 作用

1. 函数内部的变量在函数执行过后,仍然存在于内存, 延长了变量生命周期
2. 让函数外部可以操作到函数内部数据 

## 特性

1. 函数执行之后，内部变量一般不存在，只有闭包中的变量才常驻内存
2. 函数外部不能直接访问内部变量，但是可以通过闭包实现getter，setter

## 生命周期

产生：在嵌套内部函数定义的时候就产生了（不是调用时）
死亡：嵌套的内部函数不可达时变成垃圾对象

## 应用：定义JS模块

+ 将所有的数据和方法封装在一个函数内部，是私有的
+ 向外暴露一个包含n个方法的对象或者函数
+ 模块的使用者只能通过暴露对象获取方法来读写 

## 缺点

### 内存泄漏

**常见的内存泄露**：意外的全局变量、没有及时清理的计时器与回调、闭包
**双刃剑**：函数执行后，因为有闭包，函数局部变量没有释放，占用内存，如果不需要闭包了还忘记释放，会造成内存泄漏
**解决方法：**用完了之后， f = null，清除对闭包的引用，让内部函数称为垃圾对象，从而回收闭包





