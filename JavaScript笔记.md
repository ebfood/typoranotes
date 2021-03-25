## 作用域，闭包

### 什么是闭包？	

[闭包](https://en.wikipedia.org/wiki/Closure_(computer_programming)) 是指内部函数总是可以访问其所在的外部函数中声明的变量和参数，即使在其外部函数被返回（寿命终结）了之后。在 JavaScript 中，所有函数都是天生闭包的, 也就是说：JavaScript 中的函数会自动通过隐藏的 `[[Environment]]` 属性记住创建它们的位置(指向外部词法环境)，所以它们都可以访问外部变量。

### 垃圾收集

通常，函数调用完成后，会将词法环境和其中的所有变量从内存中删除。因为现在没有任何对它们的引用了。与 JavaScript 中的任何其他对象一样，词法环境仅在可达时才会被保留在内存中。

但是，如果有一个嵌套的函数在函数结束后仍可达，则它将具有引用词法环境的 `[[Environment]]` 属性， 所以，词法环境仍然是可达的，所以嵌套函数还是有效的。

### 实际情况

在实际中，JavaScript 引擎（比如v8）会试图优化它。它们会分析变量的使用情况，如果从代码中可以明显看出有未使用的外部变量，那么就会将其删除。

### 获取新的内容

Q: *外部变量改变了, 获得到最新的吗?*
A: 会, 相当于存了指针啊,[[environment]]保存了声明时外部词法环境的**引用**, 依次向外根据指针找词法环境, 这个时候已经是修改的值了.

### 词法空间之间独立性

Q: 词法空间之间互相独立吗?
A: 

```javascript
function f() {
  let value = Math.random();
  return function() { alert(value); };
}

// 数组中的 3 个函数，每个都与来自对应的 f() 的词法环境相关联
let arr = [f(), f(), f()];
arr[0](); // 0.24836736918295954
arr[1](); // 0.53527045947319736
arr[2](); //0.46883183219295654
```

### 死区

```JavaScript
function func() {
  // 引擎从函数开始就知道局部变量 x，
  // 但是变量 x 一直处于“未初始化”（无法使用）的状态，直到结束 let（“死区”）
  // 因此答案是 error

  console.log(x); // ReferenceError: Cannot access 'x' before initialization

  let x = 2;
}
```



## 原型



## 事件代理/委托



## 错误处理/异常处理



## 调度: setTimeout 和 setInterval

等待特定一段时间之后再执行就是所谓的“计划调用（scheduling a call）”。

目前有两种方式可以实现：

- `setTimeout` 允许我们将函数推迟到一段时间间隔之后再执行。
- `setInterval` 允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以该时间间隔连续重复运行该函数。

### 嵌套setTimeout

```javascript
let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);
```

可以实现和setInterval类似的效果，唯一的区别是：
setInterval:

<img src="/Users/ebcode/Library/Application Support/typora-user-images/image-20210321113812935.png" alt="image-20210321113812935" style="zoom: 50%;" />

嵌套setTimeout：
<img src="https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210321113935317.png" alt="image-20210321113935317" style="zoom:50%;" />



## 装饰器 call/apply 防抖 节流

**装饰器** 是一个围绕改变函数行为的包装器。

- [func.call(context, arg1, arg2…)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Function/call) —— 用给定的上下文和参数调用 `func`。
- [func.apply(context, args)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) —— 调用 `func` 将 `context` 作为 `this` 和类数组的 `args` 传递给参数列表。

### 防抖装饰器

1. 每一次事件被触发，都会清除当前的`timer`然后重新设置超时并调用。这会导致每一次高频事件都会取消前一次的超时调用，导致事件处理程序不能被触发

2. **只有当高频事件停止**，最后一次事件触发的超时调用才能在`delay`时间后执行

   `debounce` 就像一个“接听电话”的秘书，**直到等到**有 `ms` 毫秒的安静时间之后，才肯将**最新**的呼叫信息传达给“老板”（调用实际的 `f`）。

```javascript
function debounce(f, timeout) {
  let timer;  // 维护一个timer
  return function() {
    clearTimeout(timer);  // 打断当前调度
    timer = setTimeout(() => {  // 重新设置调度
      f.apply(this, arguments);
    }, timeout);
  }
}
```

### 节流装饰器

1. `throttle` 就像接电话的秘书，但是打扰老板（实际调用 `f`）的频率不能超过每 `ms` 毫秒一次。
2. 下面的节流装饰器特意保存并且处理最后一次调用

```javascript
function throttle(func, ms) {
  let isThrottled = false,
    savedArgs,
    savedThis;
  function wrapper() {
    if (isThrottled) { // (2)
      savedArgs = arguments;
      savedThis = this;
      return;
    }
    func.apply(this, arguments); // (1)
    isThrottled = true;
    setTimeout(function() {
      isThrottled = false; // (3)
      if (savedArgs) {
        wrapper.apply(savedThis, savedArgs);
        savedArgs = savedThis = null;
      }
    }, ms);
  }
  return wrapper;
}
```



## Promise



## Date

- 月份从 0 开始计数（对，一月是 0）。
- 一周中的某一天 `getDay()` 同样从 0 开始计算（0 代表星期日）。
- 当设置了超出范围的组件时，`Date` 会进行自我校准。如果计算100min之后的日期，可以`date.setMinutes(date.getMinutes() + 100);`
- 日期可以相减，得到的是以毫秒表示的两者的差值。
- 使用 `Date.now()` 可以更快地获取当前时间的时间戳。



## JSON

### 规则

+ 不允许单引号
+ 不允许注释
+ 不允许js表达式
+ key带双引号

### JSON.stringify

+ 忽略 function symbol undefined

+ 第二个可选参数接受一个函数，可以过滤，支持嵌套

  ```javascript
  JSON.stringify(meetup, function replacer(key, value) {
    return (key == 'occupiedBy') ? undefined : value;
  });
  ```

### toJSON

可以给对象自定义这个函数， 返回值就会在stringify中自动变换了。

### JSON.parse

第二个参数支持自定义返回值，可以深入嵌套中

```javascript
JSON.parse(schedule, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});
```



## Ajax/Fetch

## 

## 正则表达式

