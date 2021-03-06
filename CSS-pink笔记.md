# 字体

## font

font: font-style font-weight font-size/font-hight font-family

+ 顺序不能颠倒
+ 必须保留font-size font-family  eg: font 20px 'yahei'

# 文本

文本外观，颜色，对其，缩进，间距等等

## 对齐 text-align

只能设置水平对齐，是在盒子内对齐。

## 装饰文本 text-decoration

## 文本缩进 text-indent

text-indent  2em

## 行高 line-height

![image-20210306083614739](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210306083614739.png)

从第一行的最下沿到第二行的最下沿就是行高

# Emment语法

+ 批量 div*10 + tab
+ 包含> div>span + tab
+ 兄弟 + 
+ .classname + tab 默认div  p.classname 就是<p class='classname'> </p>
+ 自增符号$  eg: .classname$*5 就是自增排序了
+  {}可以添加内容 比如 div{pig}*5 就是五个内涵pig的div  
+ 混合${} eg: div{$}*5
+ css 可以w100 + tab = width: 100px;

 

# 复合选择器

## 后代选择器

 父 子 孙 ... {

}
eg: .nav li a { color:  red;}

## 子选择器

最近一级子元素，亲儿子，孙子都不行
父 > 子 {

 }

## 并集选择器

a, b, c, ... {

}

eg:  1. 喜欢竖写 2.最后一组不需要逗号 
div, 
p , 
.pig li {

}

# 伪类选择器

## 链接

+ a:link 未访问过的链接
+ a:visited 访问过的
+ a:hover 鼠标悬停
+ a:active 鼠标正在按下还没有抬起来的   
+ 需要按照lvha的顺序来定义才有效果

## :focus

选取获得焦点的表单

input:focus {
}

# 块元素

+ 可以指定高度宽度
+ 没有置顶宽度就和父亲一样宽
+ 文字块级元素，h,p里面不能再放div了，会出问题

# 行内元素

+ 宽和高设置无效
+ 只能放文本和行内，放块元素要出错

# 行内块元素

+ input img td
+ 在一行上，但是之间有空白缝隙
+ 默认宽度就是本身内容宽度
+ 宽度高度，内外边距都可以控制

# 行内 块 互相转化

display: block | inline | inline-block;

# snipaste

前端神器，我太爱了

# 行高

line-height 等于盒子的高度，居中，大于盒子高度，偏下，小于盒子的高度，偏上

# CSS背景

## background-color

## background-image

+ <u>**装饰性图片** **logo 超大图片用背景图片**</u>
  background-image: url( );

+ background-repeat 背景平铺，默认平铺，可以选择no-repeat

+ 背景颜色和背景图片可以混用，图片在上层

## **background-positon** 

后面可以根方位名词和精确单位

  1. 如果是方位名词，background-position *center top，两个值前后无关，顺序可以颠倒*
     *省略一个，默认居中*
  2. 如果是精确单位，第一个x，第二个y
     *如果省略一个，默认给定的是x，剩下的一个居中*
  3. 可以混合方位名词和精确单位, 第一个x，第二个y
     *background-position: 20px center;*

4. 超大背景图片用这个可以保证居中，直接给body一个bp属性，而且屏幕越大，看到的越多，但是中心核心内容都可以保证
   `body {
      background-image: url(...);
      background-position: center top;
   }`

## 背景固定 background-attachment

可以 **视差滚动** qq官网

background-attachment: scroll | fixed;

## 复合写法 background

background: 颜色 图片地址 平铺 滚动 位置
这个是约定俗成的顺序 

## 背景半透明

background: rgba(0, 0, 0, .5);