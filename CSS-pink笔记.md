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

line-height 等于盒子的高度，居中，大于盒子高度，偏下，小于盒子的高度，偏上

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

# 元素显示模式

## 块元素

+ 可以指定高度宽度
+ 没有置顶宽度就和父亲一样宽
+ 文字块级元素，h,p里面不能再放div了，会出问题

## 行内元素

+ 宽和高设置无效
+ 只能放文本和行内，放块元素要出错

## 行内块元素

+ input img td
+ 在一行上，但是之间有空白缝隙
+ 默认宽度就是本身内容宽度
+ 宽度高度，内外边距都可以控制

## 行内 块 互相转化

display: block | inline | inline-block;

# snipaste

前端神器，我太爱了，可以截图，挂桌面，量px，取颜色

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



# css 三大特性

## 层叠性

就是覆盖冲突的样式，样式冲突，就近原则。

## 继承性

+ 比如继承body的字体，颜色
+ text- font- line- color 这些可以继承，高度，内外边距不可以继承
+ 行高的继承，如果父亲设置行高1.5 那子元素的行高就是子元素字体大小的1.5倍
  body{ font: 12px/1.5 'YaHei'} 就是子元素行高是字体的1.5

## 优先级

继承/*0000	元素0001 	类/伪类 0010	ID0100 	style="" 1000	!important  inf
继承的权重是0， 不管他爸选择器权重多高，所以继承的干不过浏览器默认样式

### 权重叠加

ul li {} 的权重 0, 0, 0, 1+ 0, 0, 0, 1
.nav li {} 0, 0, 1, 0 + 0, 0, 0, 1 
a:hover 11
**叠加不会进位**

# 盒子模型

## 盒子模型

margin border padding content

### border

+ border: 1px solid red; 没有固定顺序
  border-top: 2px solid red;
  border-collapse: collapse; 合并相邻边框，应用场景：表格
  **border会增加盒子的实际大小，需要减去**

### padding

+ padding v1
  padding v1 v2
  padding v1 v2 v3 左右v2，上1下3
  **padding 也会影响盒子的实际大小，要计算好content的width和height，防止吧其他盒子送走** 
+ **tips** 可以利用padding撑开盒子，设置导航栏按钮，解决按钮内字数不同的问题，这样每个按钮之间距离看起来就是相等的
  ![image-20210308140736111](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210308140736111.png)
+ 在没指定width/height的时候padding不会撑开盒子大小，就是没设置宽度，继承了爸爸的宽度，然后padding就会往里面挤，如果设置了宽度，那padding就会往外扩张
  （问：那那个a是咋回事呢? 猜测可能是inline-block）

### margin

+ computed可以看到参数
+ 盒子水平居中 margin: 0 auto;
  对于行内/行内块元素，则需要在他的父亲text-align: center
+ 父盒子里面套子盒子，当子盒子设置了垂直margin，父盒子就会**塌陷**。
  + 只要父亲有边框就不会塌陷
  + 父亲来个内边距
  + overflow: hidden;
+ 清除内外边距` * {margin:0; padding:0}`
+ 行内元素很大情况下上下外边距不起作用

## 圆角边框

border-radius: 圆的半径 |  百分比; 

+ 圆形：先准备一个正方形 border-radius: 50%;
+ 胶囊：高度的一半

## 盒子阴影

box-shadow: h-shadow v-shadow blur spread color inset;

+ h-shadow影子的左右
+ blur 虚实
+ spread 大小
+ 默认外阴影 可选inset

## 文字阴影

同上 但是只有前四个值



# 浮动

CSS的三种传统布局方式是 标准流，浮动，定位，网页会由三种方式一起布局
**网页布局第一准则：多个块级纵向排列找标准流，横向排列找浮动**

**第二准则：先设置盒子大小，再设置盒子位置**

## 浮动特性

+ 1. 脱离标准流的控制，移动到指定位置，称作**脱标**
  2. 萝卜不再保留原先的坑
+ 如果多个盒子都浮动，会按照**一行内顶端对齐显示**，中间没有空隙
+ **任何元素如果浮动会给予行内块元素的特性**，比如，块级盒如果没有设置宽度，默认和爸爸一样宽，但是浮动后，就由内容决定

## 浮动搭配标准流父亲盒子

一般，先用标准流父亲上下排列，然后在盒子内采用浮动排列左右位置。

## 浮动注意点

浮动的盒子只会影响盒子后面的标准流，不会影响前面的标准流。 



## 清除浮动

在父盒子没指定高度的时候，子盒子设置了浮动，那么父盒子高度会变成0
清除浮动就是清除浮动造成的影响，爸爸可以根据孩子自动撑开高度,清除浮动的策略就是**闭合浮动**, 只让浮动在父盒子里面有影响.
clear: both;

1. **额外标签法**：最后一个孩子添加一个class, 然后css中选择并且添加 clear: both;
   这个最后的盒子必须是块级元素

2. **父亲添加overflow**

3. **after伪元素**
   在css添加如下代码

   ```css
   .clearfix:after {
   	content: "";
   	display: block;
   	height: 0;
   	clear: both;
   	visibility: hidden;
   }
   
   .clearfix {
   	/* IE6、7 专有 */
   	*zoom: 1;
   }
   ```

   然后在父元素上添加clearfix类

4. **双伪元素**(推荐)

   ```css
   .clearfix:before,.clearfix:after {
   	content: "";
   	display: table;
   }
   
   .clearfix:after {
   	clear: both;
   }
   
   .clearfix {
   	*zoom: 1;
   }
   ```



# PS切图

## 图层切图

右键图层, 导出为png
很多时候需要合并图层在导出, 先选中, 然后合并,然后在导出

## 切片切图

先切片, 文件, 导出, web格式, 选格式, 存储, 选中的切片, 保存
如果想要透明, 就把背景隐藏掉
切片选择工具可以移动切片位置, 选中切片按delete删除切片

## cutterman切图

官网下载安装, 窗口  扩展功能  cutterman
直接选中,导出选中图层
如果是多个图层, 那就合并导出选中图层 



# 学成在线案例

## CSS属性书写顺序(重要 )

![image-20210309103409382](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210309103409382.png)

## 布局思路

1. 确定版心, 确定宽度
2. 确定行模块, 用标准流
3. 确定列模块, 用浮动
4. 先写结构, 再写样式

## 导航栏思路

实际开发中,不会直接用链接a而**用li 包含at的做法**, 这是个professional的做法
如果直接用a, 搜索引擎会觉得有关键字堆砌,降低网页权重



# 定位

可以让盒子在盒子中自由飞翔, 或者固定在屏幕的某个位置的时候用定位
**定位 = 定位模式 + 边偏移**

## static

默认, 没有定位, 没有边偏移

## relative

1. 相对于原来位置的定位进行边偏移
2. **不脱标**萝卜原来的坑会被保留, 因此很大程度上的用途是给绝对定位当爸爸

## ablolute

相对于祖先元素的绝对定位

1. 如果没爸爸, or爸爸没有定位, 那document就是爸爸
2. 祖先元素有定位(只要不是static), 那就是对于祖先的绝对定位, 祖先是爸爸, 爷爷, 都行, 最近的一个. 
3. 脱标, 萝卜坑不再保留

## fixed

以浏览器可视窗口为参照固定定位

1. 和父亲没有关系, 也不随着滚动条滚动
2. 脱标, 不留坑

## sticky

大部分的效果都是js, 而不是这个, 这个兼容性不好

1. 浏览器窗口为参照物
2. 不脱标, 留坑
3. 必须添加top left right bottom其中的一个

## 子绝父相

当子采用绝对定位的时候, 需要父亲也拥有定位, 对于父亲来说, 拥有定位之后还要保留原来的位置, 于是父亲应当拥有相对定位.

## fixed贴版心算法

left: 50%;
margin-left: 版心的一半;

## 定位次序 z-index

默认auto, 越大越靠上, 但是不能加单位
只有定位的盒子才有此参数

## 绝对定位居中

left: 50%;
margin-left: -盒子宽度的一半;

top: 50%;
margin-top: -高度的一半;

## 定位特性

1. 如果添加了absolute或者fixed, 就可以直接设置宽度无需转block
2. 如果块元素加了定位, 还没给宽度高度, 那就是内容的宽高
3. 脱标的盒子不会引起margin塌陷
4. float不会压住标准流的文字, positon连文字都一起压住



# 精灵图

减少服务器请求次数, 加快页面加载速度.

+ 针对于背景图片

+ CSS二倍精灵图使用方法https://blog.csdn.net/weixin_39295546/article/details/104709129

+ 二倍精灵图,所以用ps缩放一半

  ![image-20210311151040935](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210311151040935.png)

  ![image-20210311151130063](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210311151130063.png)

  窗口 -> 信息 就可以看到当前鼠标坐标

  ![image-20210311151223940](https://ebcode.oss-cn-shanghai.aliyuncs.com/img/image-20210311151223940.png)

  然后写代码



# CSS3

## 属性选择器

+ E[att]
+ **E[att=val]**
+ E[att^=val] 属性att以val开头的元素
+ E[att$=val] .....结尾.....
+ E[att*=val] ......含有....
+ 属性选择器权重10

## 结构伪类选择器

+ E:first-child	E:last-child 是第一个孩子的E元素

+ E:nth-child(n) 是第n个孩子的E元素
   n可以是 even, odd
  也可以是n, 只能是字母n, 就是选择了所有孩子
  2n: 2x0 2x1 2x2 2x3 即偶数孩子
  5n: 5的倍数
  n+5: 从第五个到最后
-n+5: 前5个
  
+ E:first-of-type

  ```css
  div:nth-child(1) {
  	/*先找指定序号, 再去看是不是指定元素*/
  }
  div:nth-of-type(1) {
  	/*先找指定元素, 再去找序号*/
  }
  ```

  

## 伪元素选择器

通过CSS插入元素, 避免html标签嵌套, 叫伪元素是因为在文档树中找不到这个元素

### element::before | element::after

+ 在element里面的内容的前面/后面加上盒子
+ 必须有content元素
+ 是行内元素
+ 权重是1



## 渐变颜色

background: -webkit-linear-gradient(left, red, blue)
必须添加私有前缀

