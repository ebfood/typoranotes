# 去哪儿网vue练习-首页篇

## 初始化

+ `<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">`

+ main.js 引入 reset.css
+ 引入border.css 解决边框像素问题
+ cnpm install fastclick --save 解决300ms延迟
+ cnpm install stylus --save
+ cnpm install stylus-loader@3 --save **这是个坑，最新版本报错的**

## 完成Header.vue

这里用到了rem，移动端一般用rem，样图是二倍的，这样设置font-size 50px 正好抵消

### iconfont

添加项目打包下载，需要

![image-20210220095322494](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210220095322494.png)

复制，然后改一下iconfon.css的路径，就可以直接在项目里添加class='iconfont' 然后在网页里复制代码直接填写到html里面就可以使用图标

### 使用styl设置全局变量

```css
@import "~@/assets/styles/variables.styl"
```

但是太麻烦了

给styles文件夹一个别名

在webpack.base.conf.js

```javascript
'styles': resolve('src/assets/styles')
```

这样直接用styles

```css
@import "~styles/variables.styl"
```

**修改了webpack配置项要重启服务**



## 完成首页轮播图

### 创建新的git分支

在github创建分支，然后在本地pull一下，接着git checkout xxxxxx切换到新的分支，一般开发新功能要用新的分支

### Vue awesome swiper

https://github.com/surmon-china/vue-awesome-swiper

用的2.6.7

保持比例的方法

```css
.wrapper
  overflow hidden
  width 100%
  height 0
  padding-bottom 31.25%
```

### 踩坑，vue-awesome-swiper

添加小圆点，轮播，循环，都在data设置就好了

```javascript
data () {
  return {
    swiperOption: {
      pagination: {
        el: '.swiper-pagination',
        type: 'bullets'
      },
      slidesPerView: 1,
      autoplay: {
        delay: 3000,
        disableOnInteraction: false
      },
      loop: true
      // Some Swiper option/callback...
    },
    list: [{
        id: '0001',
        imgUrl: 'https://gw.alicdn.com/imgextra/i2/O1CN01wRZZPQ1uUpI69CVeo_!!6000000006041-2-tps-1125-352.png_790x10000.jpg_.webp'
      }, {
        id: '0002',
        imgUrl: 'https://gw.alicdn.com/imgextra/i2/O1CN014s2CWQ1akabgfcFul_!!6000000003368-0-tps-702-200.jpg_790x10000Q75.jpg_.webp'
      }]
  }
}
```

![image-20210220210040774](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210220210040774.png)

因为去哪儿早已经改版了，这个轮播图已经不好找了，随便找了个代替，结果发现并没有pagination显示，尽管我试了无数版本的swiper。。。

突发奇想是不是尺寸问题于是改了图片尺寸

![image-20210220210236779](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210220210236779.png)

有了呗，就是你的尺寸显示不全 hidden给弄没了。。。我的一下午啊



### 防止页面抖动，需要先撑起来div

```stylus
.wrapper
  overflow hidden
  width 100%
  height 0
  padding-bottom 80%
  background #eee
```

### 样式穿透

轮播下面的小点想要变颜色，由于类不在这个组件中，而是在其他组件中(与轮播图插件有关)：

```stylus
.wrapper >>> .swiper-pagination-bullet-active
  background #fff
```

## 完成首页图标导航

### 布局

首先创建div，撑开，保持比例

```stylus
.icons
    width 100%
    height 0 /* 限制高度*/
    padding-bottom 50%
    background green
```

在下面撑开一个图标div,比例都是25，正好可以放八个

```stylus
.icon
  position relative
  float left
  width 25%
  height 0
  padding-bottom 25%
  background red
```

然后在小方盒子里面分区，一个图像区域，一个文字区域，需要吧position设置absolute

图像的

```stylus
.icon-img
        position absolute
        top 0
        left 0
        right 0
        bottom .44rem
        background blue
        box-sizing border-box
        padding: .15rem
        .icon-img-content
          height 100%
          //居中
          display block
          margin 0 auto
```

文字的

```stylus
.icon-desc
  position absolute
  line-height  .44rem
  left 0
  right 0
  bottom 0
  text-align center
```

![image-20210220225411216](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210220225411216.png)



### 逻辑

先把数据都存在data

```vue
{
  id: '0001',
  imgUrl: 'https://picbed.qunarzz.com/01d2f57f920666364197a850dab859a8.png',
  desc: '民宿客栈'
}
```

接着就可以用for循环来显示

会有一个扔在了外面

![image-20210220233958090](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210220233958090.png)



#### 用一个swiper实现图标换页

首先实现pages的computed

```vue
computed: {
  pages () {
    // 列表分页，八个一组
    const pages = []
    this.iconList.forEach((item, index) => {
      const p = Math.floor(index / 8) // 页码
      if (!pages[p]) pages[p] = []
      pages[p].push(item)
    })
    return pages
  }
}
```

然后下载chrome的 vue dev tool就可以查看到pages是一个二维数组，第一页八个元素，第二页一个元素

![image-20210220235210076](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210220235210076.png)

**重点来了，v-for的二层循环**

+ 第一层, for的是computed的pages，循环项取名page

  ```html
  <swiper-slide v-for="(page,index) of pages" :key="index">
  ```

+ 第二层 还是原来的div，不过这次for的是page，完美的把行程设计放到了第二页

  ```html
  <div class="icon" v-for="icon of page" :key="icon.id">
  ```



#### 当字符串过长实现省略...

创建一个mixins.styl

```stylus
ellipse()
  overflow hidden
  white-space nowrap
  text-overflow ellipsis
```

@import， 调用

![image-20210221000532254](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210221000532254.png)

## 热门推荐组件

### html css

```vue
<template>
  <div>
    <div class="header">热门推荐</div>
    <ul>
      <li class="item" v-for="item of itemList" :key="item.id">
        <img class="item-img" :alt="item.title" :src="item.imgUrl">
        <div>
          <p class="item-title">{{ item.title }}</p>
          <p class="item-desc">{{ item.desc }}</p>
          <button class="item-button">查看详情</button>
        </div>
      </li>
    </ul>
  </div>
</template>
```

+ 用flex布局，然后一个图片，一个div里面装着title，desc，button

+ 关于图片

  ```stylus
  .item-img
    width 1.7rem
    height 1.7rem
    padding .1rem
  ```

+ 一像素边框`class="item border-bottom"`

+ 文字超出边界

  上文的`ellipse()` 还要在父亲div中`min-width 0`



## ajax axios

cnpm install axios --save

在methods里面定义请求函数，并且在主页的mounted上调用，实现一次ajax请求

在项目中，只有static文件夹可以被外部访问，因此需要在其中创建mock文件夹作为模拟数据

为了上线之前不对接口改动，需要用转发机制，运用webpack的代理功能

config/index.js 中的

```javascript
proxyTable: {
      '/api': {
        target: 'http://localhost:8080',
        pathRewrite: {
          '^/api' : '/static/mock'
        }
      }
    },
```

然后就可以用api作为接口了

```vue
axios.get('/api/index.json')
  .then(this.getHomeInfoSucc)
```

这样我们请求的api会被webpack-dev-server转发到mock文件夹中

接着就是父子组件传递数据

定义data

```javascript
data () {
  return {
    city: '',
    swiperList: [],
    iconList: [],
    recommendList: [],
    weekendList: []
  }
},
```

然后在请求成功赋值

```javascript
getHomeInfoSucc (res) {
  res = res.data
  if (res.ret && res.data) {
    const data = res.data
    this.city = data.city
    this.swiperList = data.swiperList
    this.iconList = data.iconList
    this.recommendList = data.recommendList
    this.weekendList = data.recommendList
  }
}
```

模版里给子组件传值

```html
<home-header :city="city"></home-header>
<home-swiper :list="swiperList"></home-swiper>
<home-icons :list="iconList"></home-icons>
<home-recommend :list="recommendList"></home-recommend>
<home-weekend :list="weekendList"></home-weekend>
```

在子组件Swiper.vue（举个例子）

首先肯定要接受参数，删除之前写死的data变量，在模版里把变量改了。

这个组件有点特殊 swiper 的轮播导航点点，对又是他，会根据数组长度来构建，我们开始传入的数据len是0，就bug了，于是创建一个computed，当ajax传入成功的时候自动计算，然后把这个变量绑定在swiper组件上v-if就好了，这样的当length 不等于0的时候才构建组件

```vue
props: {
  list: Array
},
computed: {
    showSwiper () {
      return this.list.length
    }
  }
```

首页结束，合影留念

![image-20210222114138020](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210222114138020.png)

