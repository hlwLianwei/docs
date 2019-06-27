# css-flex样式相关
主要是一些flex样式说明

## flex弹性布局
### 1.基本显示
- wxml(微信小程序页面)
```
<view class="box">
  <view class='item'>item1</view>
  <view class='item'>item2</view>
  <view class='item'>item3</view>
</view>
```
- wxss(微信小程序样式)
```
.box{
  display: flex;
  flex-direction: row;/* 主轴方向为row, 即子元素水平左向右排序 */
}
.item{
  width: 200rpx;
  height: 200rpx;
  background: #089e3a9f;
  margin: 12rpx;
}
```
- 显示如下：
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_01.jpg)

### 2.容器的属性
- 采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。
- 容器默认存在两根轴：主轴(main axis)和交叉轴(cross axis)(与主轴垂直交叉)，主轴方向即可以是水平方向，也可以是竖直方向。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end, 交叉轴的开始位置叫做cross start，结束位置叫做cross end。
- 项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
- 以下6个属性设置在容器上:
```
/* 决定主轴的方向（即子元素的排列方向） */
flex-direction
/* 默认子元素都排在一条线上。如果排不下，确定如何换行 */
flex-wrap
/*flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。*/
flex-flow
/*justify-content属性定义了子元素在主轴上的对齐方式。*/
justify-content
/*align-items属性定义子元素在交叉轴上如何对齐。*/
align-items
/* align-content属性定义了多根轴线的对齐方式。如果元素内只有一根轴线，该属性不起作用。*/
align-content
```

#### 2.1 flex-direction属性
```
.box {
  /* 决定主轴的方向（即子元素的排列方向） */
  flex-direction: row | row-reverse | column | column-reverse;
}
```
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_02.jpg)

#### 2.2 flex-wrap属性
```
.box{
  display: flex;
  flex-direction: row;
  /* 默认子元素都排在一条线上。如果排不下，确定如何换行 */
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_03.jpg)
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_04.jpg)
    nowrap（默认）：不换行。
    wrap：换行，第一行在上方。
    wrap-reverse：换行，第一行在下方。 

#### 2.3 flex-flow属性
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
```
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

#### 2.4 justify-content属性
```
.box {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;  
  /* 定义子元素在主轴上的对齐方式 */
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
    flex-start（默认值）：子元素左对齐
    flex-end：子元素右对齐
    center： 子元素居中
    space-between：子元素两端对齐，子元素之间的间隔都相等。
    space-around：每个子元素两侧的间隔相等。所以，子元素之间的间隔比子元素与边框的间隔大一倍。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_05.jpg)

#### 2.5 align-items属性
```
.box {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
  /* 定义子元素在交叉轴上如何对齐 */
  align-items:flex-start | flex-end | center | baseline | stretch;
}
```
    flex-start：交叉轴的起点对齐。
    flex-end：交叉轴的终点对齐。
    center：交叉轴的中点对齐。
    baseline: 项目的第一行文字的基线对齐。
    stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_06.jpg)

#### 2.6 align-content属性
```
.box {
  /* 属性定义了多根轴线的对齐方式。如果元素内只有一根轴线，该属性不起作用。 */
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
    flex-start：与交叉轴的起点对齐。
    flex-end：与交叉轴的终点对齐。
    center：与交叉轴的中点对齐。
    space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
    space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
    stretch（默认值）：轴线占满整个交叉轴。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_07.jpg)

### 3.容器子元素的属性 
- 以下6个属性可以设置在子元素上：  
```
/* 定义子元素的排列顺序。数值越小，排列越靠前，默认为0。 */
order
/* 定义子元素的放大比例，默认为0，即如果存在剩余空间，也不放大。 */
flex-grow
/* 定义子元素的缩小比例，默认为1，即如果空间不足，该子元素将缩小。  */
flex-shrink
/* 定义了在分配多余空间之前，子元素占据的主轴空间(main size)。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即子元素的本来大小。 */
flex-basis
/* 是flex-grow, flex-shrink和flex-basis的简写，默认值为0,1,auto。后两个属性可选。 */
flex
/* 允许单个子元素有与其他子元素不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。 */
align-self
```

#### 3.1 子元素 order属性
```
.item.it1{
  /* 定义子元素的排列顺序。数值越小，排列越靠前，默认为0。 */
  order:1; /* default 0 */
}
```
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_08.jpg)

#### 3.2 子元素 flex-grow属性
```
.item.it1{
  /* 定义子元素的放大比例，默认为0，即如果存在剩余空间，也不放大。 */
  flex-grow:1; /* default 0 */
}
```
如果所有子元素的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个子元素的flex-grow属性为4，其他子元素都为1，则前者占据的剩余空间将比其他子元素多三倍。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_09.jpg)

#### 3.3 子元素 flex-shrink属性
```
.item.it1{
  /* 定义子元素的缩小比例，默认为1，即如果空间不足，该子元素将缩小。*/
  flex-shrink:1; /* default 0 */
}
```
如果所有子元素的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个子元素的flex-shrink属性为0，其他子元素都为1，则空间不足时，前者不缩小。负值对该属性无效。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_10.jpg)

#### 3.4 子元素 flex-basis属性
```
.item.it1{
  /* 定义了在分配多余空间之前，子元素占据的主轴空间(main size)。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即子元素的本来大小。*/
  flex-basis:350rpx; /* default auto */
}
```
它可以设为跟width或height属性一样的值（比如350rpx），则子元素将占据固定空间。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_11.jpg)

#### 3.5 子元素 flex属性
```
.item.it1{
  /* 是flex-grow, flex-shrink和flex-basis的简写，默认值为0,1,auto。后两个属性可选*/
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
- 该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
- 建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_12.jpg)

#### 3.6 子元素 align-self属性
```
.item.it1{
  /* 允许单个子元素有与其他子元素不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。 */
 align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```
- 该属性可能取6个值，除了auto，其他都与align-items属性完全一致。
![](https://raw.githubusercontent.com/hlwLianwei/docs/master/H5/images/flex_13.jpg)
