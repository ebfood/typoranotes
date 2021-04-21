# 使用

## 组件之间传值

### 父子之间

父对子：props  子对父 this.$emit

### 非父子之间传值

- 借助Vue官方提供的一个数据层的框架——**Vuex**
- **发布-订阅模式**（又称为：总线机制/Bus/观察者模式)

## 生命周期钩子

**created：**data和 method好了，初始化工作，无法访问dom，可以做一些异步请求
**mounted：**dom挂在完成了，一些需要dom的库，ajax发送，定时器启动，事件的监听，订阅者在这里写
**updated：**获取到更新后的dom，依赖于dom的库需要知道状态更新在这写
**destoryed：**取消定时器，window事件解绑操作，事件监听器解绑

### created和mounted的区别

created只是vue实例初始化但是页面还没有开始渲染，mounted是dom已经渲染完毕

### 父子组件的生命周期顺序

创建从外到内，渲染从内到外，也就是儿子渲染完了爸爸才算完，mounted，updated，destoryed同理 

## Vue高级特性

### 自定义v-model

v-model是语法糖

```html
<input type="text" v-model="name">
```

相当于

```html
<input type="text" :value="name" @input="name = $event.target.value">
```

所以在自定义组件里面想使用v-model的话：

1. 组件添加一个model属性，同时在props属性添加变量

   ```js
   	model: {
       prop: 'checked', // 这个要对应props里面的
       event: 'change'
     },
     props: {
       checked: Boolean
     }
   ```

2. 在模版里这样去写，就是实现v-model的原理

   ```html
   <input
         type="checkbox"
         v-bind:checked="checked"
         v-on:change="$emit('change', $event.target.checked)"
   >
   ```

3. 然后就可以在组件愉快的v-model

   ```html
   <base-checkbox v-model="lovingVue"></base-checkbox>
   ```

   

### $nextTick

+ data改变之后异步渲染dom，$nextTick 就是DOM渲染完成之后的回调
+ 页面渲染时会吧data的修改做整合，多次data修改只会渲染一次

```JavaScript
this.$nextTick(()=>{
	// DOM异步渲染完成的回调函数
})
```

### slot

#### 作用域插槽

父组件希望访问到插槽组件的作用域

+ 在<slot>中要定义一个动态的属性 比如:slotData绑定在自己相应的data属性上
+ 回到外层在组件里面定义<template>, 添加v-slot属性, 随便取个名字吧,这里取slotProps 然后就可以在<template>中用插值表达式访问插槽组件的作用域里面刚刚绑定的属性了slotProps.slotData?.xxx

#### 具名插槽

多个插槽，防止插乱了，给个名儿

+ <slot name="abc">

+ <template v-slot:abc>

### 动态组件

+ :is="component-name"
+ 组件类型不确定，需要用数据动态渲染组件，比如新闻页，不确定请求了多少图片，文本，渲染的时候就需要根据数据确定渲染啥组件了

### 异步组件

如果每个组件都在页面加载的时候提前全部都加载好，当组件足够多或者足够复杂的时候势必大大影响性能，那就需要异步加载组件，需要的时候在再加载。

```js
components: {
	// ...,
  Dynamic: () => import('...')
}
```

### keep-alive

+ 频繁切换但是不需要重复渲染
+ vue常见的性能优化方法
+ mounted之后就不会再destoryed

### mixin 

+ mixin的作用是抽离多个组件公共的逻辑
+ 在使用mixin的组件中引入后，mixin中的方法和属性也就并入到该组件中，可以直接使用。
+ 可能导致来源不明可读性差、命名冲突、形成多对多的关系很烦人。

## 核心插件

### Vuex

### Vue-Router

**路由模式（hash、history）**: 后者需要server端的支持

**动态路由**：动态路径参数，冒号开头，在页面中用$route.params.id取出来参数做后续操作

**懒加载：**

```js
routes: [
	{
		path: '/',
		component: () => import('...')
	}
]
```



# 原理

## MVVM

+ 传统组件只是静态渲染，更新还要依赖与操作DOM
+ 数据驱动视图不再操作dom，更加关注数据和业务逻辑

<img src="https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210329013428828.png" alt="image-20210329013428828" style="zoom: 50%;" />

## Vue响应式

+ 核心api Object.defineProperty
  ![image-20210417143631459](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210417143631459.png)

### Object.defineProterty缺点

+ 深度监听需要递归到底，一次性计算量大
+ 无法监听新增删除属性，所以要用到Vue.set，Vue.delete
+ 无法原生监听数组，需要插入一层数组原型，在这个原型上多一层操作

## 虚拟DOM和diff

JS的运行速度比渲染DOM快几个数量级，因此可以吧dom全部用js模拟，最终diff后再渲染一次dom：![image-20210417152724346](/Users/ebcode/Library/Application Support/typora-user-images/image-20210417152724346.png)

### diff算法

+ 只比较同一层级
+ tag不同直接删除，不再深度比较
+ tag+key两者相同认为是同一节点，不再深度比较

## 模版编译

+ 模版编译为render函数，执行render返回一个vnode
+ 基于vnode可以执行patch和diff
+ 使用webpack vue-loader，可以在开发环境下编译模版

## 组件 更新/渲染 过程

### 核心原理

+ 响应式：监听data的getter setter，深度监听，数组监听，核心api
+ 模版编译：模版到render，render返回vnode
+ vdom: patch(elem, vnode), patch(vnode, newVnode)

### 初次渲染过程

+ 解析模版为render（or在开发环境已经通过vue-loader渲染好了）
+ 触发响应式，监听getter setter(注意顺序，一定先触发响应式)
+ 执行render，生成vnode，patch(elem, vnode)
  注意：在这一步中，执行render会‘touch’getter，同时就会监听可能引起模版变化的值
  ![image-20210418004803754](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210418004803754.png)

### 更新过程

+ 修改data，触发setter（此前已经touch了getter）
+ 重新执行render，生成newVnode
+ patch(vnode, newVnode)

### 异步渲染

+ $nextTick 会等待DOM更新渲染后回调
+ 异步可以汇总data的修改，最后一次行更新视图
+ 减少DOM操作次数，避免频繁渲染DOM，可以提高性能



### 完整流程图

![](https://cn.vuejs.org/images/data.png)

起点是黄色，也就是模版编译，生成一个render，然后触发响应式（初次渲染），执行render后会touch的getter，property会被记录为依赖（也就是这个数据的改动和模版是关联的），这个依赖会被watcher收集起来。
当修改了数据，之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件re-render，形成回路。



# 面试题

## v-show v-if

+ v-show通过CSS display控制和隐藏
+ v-if真正的销毁和渲染
+ 频繁切换show，否则if

## 为何在for里面key

+ 必须通过key，而不是index和random
  原因：https://blog.csdn.net/z9061/article/details/105481941
+ diff中通过tag和key来判断是不是sameNode
+ 提升复用，减少渲染次数，提升渲染性能

## 组件生命周期

### 单个组件

![单个组件生命周期](https://cn.vuejs.org/images/lifecycle.png)

### 父子

**加载渲染：**
   父beforeCreate
->父created
->父beforeMount🌟
->子beforeCreate 
->子created
->子beforeMount
->子mounted
->父mounted 🌟

**子组件更新：**
父beforeUpdate->
子beforeUpdate->
子updated->
父updated

**父组件更新：**
父 beforeUpdate -> 
父 updated

**销毁：**
父beforeDestroy->🌟
子beforeDestroy->
子destroyed->
父destroyed🌟

## Vue组件如何通讯

+ props + this.$emit
+ .$on + .$emit + .$off
+ vuex



## 组件渲染和更新的全过程

https://cn.vuejs.org/v2/guide/reactivity.html



## v-model的原理

+ input元素的 :value = "this.name"
+ 绑定input事件 @input= "this.name = $event.target.value"
+ data更新出发re-render



## 为什么data必须是一个函数呢？

Object是引用数据类型，如果不用function返回，每个组件的data都是内存的同一个地址，一个数据改变了其他也改变了；
JavaScript只有函数构成作用域(注意理解作用域，**只有函数{}构成作用域**,对象的{}以及if(){}都不构成作用域),data是一个函数时，每个组件实例都有自己的作用域，每个实例相互独立，不会相互影响。

## 自定义v-model

https://cn.vuejs.org/v2/guide/components-custom-events.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E7%9A%84-v-model

## 说一下Mixin

## 异步组件

加载大组件，路由异步加载

## 何时使用beforeDestroy

+ 解除绑定自定义事件 .$off
+ 清除定时器
+ 绑定自定义dom事件



## 作用域插槽

当父亲组件想访问子组件作用域里的变量时使用作用域插槽。

1. 子组件slot元素上绑定好要访问的属性

   ```vue
   <span>
     <slot v-bind:user="user"></slot>
   </span>
   ```

   

2. 父亲组件使用template

   ```vue
   <current-user>
     <template v-slot:default="slotProps">
       {{ slotProps.user.firstName }}
     </template>
   </current-user>
   ```

   

## keepalive

https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%9C%A8%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-keep-alive



## action和mutation的区别

+ action处理异步，mutation不可以
+ mutation做原子操作
+ action可以整合多个mutation

## Vue-router常用路由模式

+ hash
+ history

## VueRouter异步加载的写法

## 请用vnode描述一个DOM结构

## 监听data的核心api？缺点？

## Vue如何监听数组变化的？

+ Object.defineProterty不能监听数组
+ 需要重新定义原型（加一层原型），重写push和pop方法实现监听
+ Proxy可原生支持监听数组

## 描述响应式原理

还是一样的答：监听data变化+组件渲染流程图
https://cn.vuejs.org/v2/guide/reactivity.html

## diff算法时间复杂度

+ O(n^3) -> O(n)
+ 只比较同一层级
+ tag不同直接删除，不再深度比较
+ tag+key两者相同认为是同一节点，不再深度比较

## 简述diff算法的过程（sdambdom）

+ patch(elem,vnode) & patch(vnode,newVnode)
+ patchVnode & addVnodes & removeVnodes
+ updateChild (key的重要性)



## Vue为何异步渲染，$nextTick

+ 异步渲染以合并data修改，提高渲染性能
+ $nextTick在DOM更新 之后触发回调

## Vue常见性能优化

+ v-show v-if，频繁切换show，否则if
+ computed缓存机制
+ v-for 加key，避免和v-if同时使用，因为vfor的优先级更高，每次for都要if，浪费了性能
+ 自定义事件和DOM事件及时销毁
+ 使用异步组件
+ 使用keep-alive
+ data层级不要太深，减少响应式监听的递归次数。
+ 使用vue-loader做好模版编译

