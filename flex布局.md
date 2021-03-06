# flex 父项的属性

**父亲必须有display: flex**

## 主轴侧轴

flex-direction: row | column
设置谁谁就是主轴

## 对齐方式 

主轴的对齐方式
justify-content: xxx

## 换行

flex默认不换行，如果装不开，缩小子元素宽度，硬塞进去

flex-wrap: warp  就可以另起一行显示

## 侧轴上的对齐方式（单行）

侧轴是相对于主轴的，默认的主轴是x
align-items: xxx

+ 拉伸： stretch，子元素不能设置高度，就会沿着y轴去拉伸
+ 对齐方式先主轴，然后侧轴
+ 只适用于单行子元素的情况

### 侧轴对齐方式（换行时）

出现多行时候才有效

+ flex-wrap: wrap; 因为有了换行，所以用align-content
+ align-content:  flex-start | center | space-between | space-around...

### 符合属性 主轴方向和换行

flex-flow: row wrap;
行主轴，而且换行

# 子项属性

## flex 剩余空间分几份

flex 1 | 2 |3 .....

 ## 在侧轴上的单独行动

align-self 控制子项在侧轴上的排列方式
align-self: flex-end;

## 排列顺序

order: <number>; 越小越靠前，默认是0