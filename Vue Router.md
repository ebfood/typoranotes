# Vue Router

## router-link

+ 添加 active-class:"xxx"; 就可以实现点击添加一个xxx类, 就可以选择器设置样式啦.
+ 相当于一个“插槽”, 是个占位容器

## 重定向

在index.js中

```javascript
{
	path: '/', // '*' 是通配符, 乱写也会到redirect
	redirect: '/film'
}
```

## 嵌套路由

index.js 对应的路由对象下添加 children属性, 在下面在添加一个路由的对象

```javascript
{
	path: '/film',
	component: Film,
	children: [
    {
			path: 'nowplaying',
			component: NowPlaying
    },
    {  //重定向
      path:'',
      redirect: '/film/nowplaying'
    }
	]
}
```

## 动态路由

编程式导航: 通过js的方式去跳转, 经典应用之列表跳详情

```javascript
// 在标签里绑定@click="handleClick(item.id)"
// 在methods里面写回调
handleClick(id) {
  this.$router.push(`/detail/${id}`)
  
  // 还可以根据命名跳转
}
```

```js
// index.js中
{
	path: 'detail/:id',
	component: Detail
}
```

只要是符合格式, 都会路由到detail
然后在Detail组件中

```js
// 组件中
mounted () {
	console.log(this.$router.params.id);
  // ajax....
}
```

## history模式

这种模式很好看, 但是要玩好, 需要后端配置好, 得告诉后端要怎么配置, 官方文档写的很详细.

## 路由守卫

https://router.vuejs.org/zh/guide/advanced/navigation-guards.html

## 懒加载

https://router.vuejs.org/zh/guide/advanced/lazy-loading.html

不再在文件开头引用模块了, 在路由的时候才写箭头函数加载

## 反向代理

解决跨域请求的问题
https://cli.vuejs.org/zh/config/#devserver-proxy



# 项目实战

## 一级路由

```js
 {
    path: '/',
    name: 'Film',
    component: Film
  },
  {
    path: '/cinema',
    name: 'Cinema',
    component: Cinema
  },
  {
    path: '/center',
    name: 'Center',
    component: Center
  }
```

## 声明式导航

在app.vue的router-view同级别声明一个导航, 来链接

```html
<div id="app">
  <router-view></router-view>
  <tabbar></tabbar>
</div>
```

## 二级路由

惯例在views文件夹下建立一个和页面名字一样的文件夹放他的孩子.
![image-20210316164054580](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210316164054580.png)



## 几种网站提供的接口

1. 后端配置好cors:  `Access-Control-Allow-Origin: *`
   这种可以直接get
2. 没有配置跨域的, 像猫眼, 那就**反向代理**, get自己的接口, 然后在vue.config.js里面配置代理, 代理到猫眼去.
3. 后端配置允许跨域, 但是需要加header验证, 需要axios请求的时候加个heade