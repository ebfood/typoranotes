# 基础

## meta视口标签

```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

取消默认的980px视口, 来用理想视口显示

## 物理像素比

在pc上1px等于屏幕上的一个像素点
在移动端, 就不一定, 一个px能显示的物理像素点的个数, 称之为物理像素比or屏幕像素比
eg: iPhone678 都是2
物理像素是750, 1px等于两个物理像素

## 二倍图

为了解决在**Retina屏幕**放大了图像倍数导致**模糊**的问题, 采用了倍图提高图像质量, 两倍于开发尺寸的图像大小, 压缩成正常大小

## 二倍精灵图

CSS使用二倍精灵图的方法https://blog.csdn.net/weixin_39295546/article/details/104709129

## background-size

+ 拉伸背景图, 只给一个参数就是宽度,
+ 也可以给百分比, 更方便
+ 值 cover是完全盖住盒子
+ contain 宽度到了就行

## 移动端开发选择

+ 单独开发移动端页面
+ 响应式兼容pc移动端 

## 移动端初始化CSS

nomalize.css

## box-sizing

box-sizing: border-box;
有了这句就让盒子变成CSS3盒子模型, border和padding不会再撑开盒子了. 

## 流式布局

+ 也就是百分比布局
+ max-width 最大宽度 min-width 最小宽度



# flex布局

## 原理

+ float、clear、vertical-align属性失效
+ 又叫伸缩布局 弹性布局
+ 采用flex称作flex容器, 成员成为flex项目
+ 通过给父亲添加flex属性,控制子盒子的位置和排列方式

## flex 父项的属性

**父亲必须有display: flex**

### flex-direction 主轴方向

flex-direction: row | column;

+ 设置谁谁就是主轴, 剩下的就是侧轴喽
+ 元素按照主轴排列

### justify-content 主轴子元素对齐方式 

+ 一定要先确认好主轴
+ flex-start 左到右 默认
+ center 居中对齐
+ space-around 平分剩余空间
+ space-between 两边贴边, 剩余平分

### flex-wrap 换行

flex默认不换行，如果装不开，缩小子元素宽度，硬塞进去
flex-wrap: warp  就可以另起一行显示

### align-items 侧轴上的对齐方式（单行）

侧轴是相对于主轴的，默认的主轴是x
align-items: xxx

+ flex-start | center | stretch(子元素不能设置高度，就会沿着y轴去拉伸)
+ 对齐方式先主轴，然后侧轴, 所以 主轴是x, 还是y, 都居中, 结果不一样
+ 只适用于单行子元素的情况

### align-content 侧轴对齐方式（换行时）

出现多行时候才有效

+ flex-wrap: wrap; 因为有了换行，所以用align-content
+ align-content:  flex-start | center | space-between | space-around...

### flex-flow 符合属性

flex-flow: row wrap;
行主轴，而且换行

## 子项属性

flex的子项行内元素产生宽度高度

### flex 剩余空间分几份

排除了已经给的width/height, 剩余的距离平均分
flex 1 | 2 |3 ..... 也可以是百分比
注意, 这个的计算方式是, 一个父亲下有三个儿子flex分别是 1, 2, 3, 那就是分别占1/6, 2/6, 3/6

 ### 在侧轴上的单独行动

align-self 控制子项在侧轴上的排列方式
align-self: flex-end;

### 排列顺序

order: <number>; 越小越靠前，默认是0





# rem布局

