---
title: 深入CSS
date: 2023-01-16 15:47:02
tags:
categories:
- [前端]
---

# CSS简介

## CSS是什么

**层叠样式表（Cascading Style Sheets, CSS）** 用来定义页面元素的样式。

## 在页面中使用CSS的三种方式

1. **外链**

   ```html
   <link rel="stylesheet" href="/assets/style.css">
   ```

2. **嵌入**

   ```html
   <style>
       li { margin: 0; }
   </style>
   ```

3. **内联**

   ```html
   <p style="margin: 1em 0">Example Content</p>
   ```



## 选择器Selector

### 单个选择器

- 通配选择器 *
- 标签选择器 h1
- id选择器 #logo
- 类选择器 .done
- 属性选择器 [disabled], [type="password"], a[href^="#"],  a[href$=".jpg"]
- 伪类选择器
  - 状态性伪类，a:link, a:visited, a:hover, a:active, input:focus
  - 结构伪类，li:first-child, li:last-child

### 组合选择器

| 名称       | 语法 | 说明                        | 示例        |
| ---------- | ---- | --------------------------- | ----------- |
| 直接组合   | AB   | 同时满足A和B                | input:focus |
| 后代组合   | A B  | 选中B，如果它是A的子孙      | nav a       |
| 亲子组合   | A>B  | 选中B，如果它是A的子元素    | section>p   |
| 兄弟选择器 | A~B  | 选中B，如果它在A后且和A同级 | h2~p        |
| 相邻选择器 | A+B  | 选中B，如果它紧跟在A后面    | h2+p        |

## 常见CSS属性

### color 颜色

三种表示形式：

- rgb(red, green, blue)，红绿蓝三个参数，简写#000000
- hsl(hue, saturation, lightness)，色相、饱和度、亮度，hsl(18, 50%, 50%)
- 颜色名字如black、white

可在最后加上透明度。

### font-family 字体

`font-family:` 一般后面跟多个字体，如果设备上没有前面的字体，则匹配之后的字体。

使用**Web Fonts**

```css
@font-face {
    font-family: "Megrim";
    src: url(https://fonts.gstatic.com/s/megrim/v11/46kulbz5wjvLqJZVam_hVUdI1w.woff2) format('woff2')
}

h1 {
    font-family: Megrim, Cursive
}
```



### font-size 字体大小

- 关键字：small、medium、large
- 长度：px、em
- 百分数：相当于父元素字体大小

### font-style 字体效果

`font-style` 默认值是normal，斜体是italic

### font-weight 字重/粗细

用100、200 …… 900表示，400等同normal，700等同bold，不同字体支持的字重不同

### line-height 行高

两行文字的base_line之间的距离

### white-space 空白符处理

- `normal` 默认情况下，多个连续的空格或换行会被合并成一个

- `nowrap` 不换行

- `pre` 保留所有空格和换行

- `pre-wrap` 保留空格，一行内自动换行
- `pre-line` 合并空格，保留换行

## 调试CSS

- 右键点击页面，选择**检查**
- 使用 快捷键
  - Ctrl + Shift + I (Windows)
  - Cmd + Opt + I (Mac)



# 深入CSS

## 选择器优先级/特异性

important > 内联 > id > (伪)类 > 标签 > 通配符

## 继承与初始值

某些属性会自动继承其父元素的计算值，除非显式指定一个值。

一般与文字相关的属性可继承，如color，而与盒模型相关的属性不被继承，如width

可通过设置inherit属性值来显式继承

## 初始值

CSS中，每个属性都有一个初始值

- background-color 的初始值为 transparent

可以使用initial关键字显式重置为初始值

- background-color: initial;

## 求值过程

1. 获取DOM树与样式规则

2. filtering

   对应用到该页面的规则用以下条件进行筛选：选择器匹配、属性有效、符合当前media等

   得到**声明值**，Declared Values，一个元素的某属性可能有0到多个声明值。比如`p {font-size:16px}`和`p.text {font-size:1.2em}`

3. cascading

   按照来源、!important、选择器特异性、书写顺序等选出优先级最高的一个属性值

   得到**层叠值**，Cascaded Value，在层叠过程中，优先级最高的值，比如1.2em

4. defaulting

   当层叠值为空的时候，使用继承或初始值

   得到**指定值**，Specified Value，经过cascading和defaulting之后，保证指定值一定不为空

5. resolving

   将一些相对值或者关键字转化成绝对值，比如em转为px，相对路径转为绝对路径

   得到**计算值**，Computed Value，一般来说是，浏览器在不进行实际布局的情况下，所能得到的最具体的值。比如60%

6. formatting

   将计算值进一步转化，比如关键字、百分比等都转为绝对值

   得到**使用值**，Used Value，进行实际布局时使用的值，不会再有相对值或关键字。比如400.2px

7. constraining

   将小数像素值转为整数

   得到**实际值**，渲染时实际生效的值，比如400px

# 布局Layout

## 布局简介

确定内容的大小和位置的算法

依据元素、容器、兄弟节点和内容等信息来计算

## 布局相关技术

- 常规流（行级、块级、表格布局、FlexBox、Grid布局）

- 浮动

- 绝对定位

### 盒模型

一个盒子尺寸包括margin、border、padding、content

#### box-sizing: content box

width

- 指定content box宽度

- 百分数相对于容器的content box宽度

height

- 指定content box高度

- 百分数相对于容器的content box高度，**容器有指定的高度时，百分数才生效**

padding

- 内边距，百分数相对于容器的**宽度**

border

- 边框，可利用边框制作三角形等图形

margin

- 外边距，百分数相对于容器的**宽度**
- margin在垂直方向上存在**margin collapse**，**边距重叠**，会取两个外边距的最大值

#### box-sizing: border-box

- width、height包含border、padding、content

### overflow

对于超出盒子大小的内容，可通过overflow的属性值来指定其显示情况

- vidible 可见
- hidden 隐藏
- scroll 滚动条

## 常规流布局 Normal Flow

- 根元素、浮动和绝对定位的元素会脱离常规流
- 其他元素都在常规流之内（in-flow）
- 常规流中的盒子，在某种**排版上下文**中参与布局

### 行级元素

span、em、strong、cite、code等

### 块级元素

body、article、div、main、section、h1-6、p、ul、li等

### 行级排版上下文 

- Inline Formatting Context (IFC)
- **只包含行级盒子**的容器会创建一个IFC
- IFC内的排版规则
  - 盒子在一行内水平摆放
  - 一行放不下时，换行显示
  - text-align决定一行内盒子的水平对齐
  - vertical-align决定一个盒子在行内的垂直对齐
  - 避开浮动(float)元素

### 块级排版上下文

- Block Formatting Context (BFC)
- 某些容器会创建一个BFC
  - 根元素
  - 浮动、绝对定位、inline-block
  - Flex子项和Grid子项
  - overflow值不是visible的块盒
  - display: flow-root;
- BFC内的排版规则
  - 盒子从上到下摆放
  - 垂直margin合并
  - BFC内盒子的margin不会与外面的合并
  - BFC不会和浮动元素重叠

### Flex Box 排版上下文

- 它可以控制子级盒子的
  - 摆放的流向
  - 摆放顺序
  - 盒子宽度和高度
  - 水平和垂直方向的对齐
  - 是否允许折行

参考文档：http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

Webkit 内核的浏览器，必须加上-webkit前缀。

```css
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```

**容器的属性**

- flex-direction
  - 决定主轴的方向（即项目的排列方向）
  - `row`（默认值）：主轴为水平方向，起点在左端
  - `row-reverse`：主轴为水平方向，起点在右端
  - `column`：主轴为垂直方向，起点在上沿
  - `column-reverse`：主轴为垂直方向，起点在下沿
- flex-wrap
  - 属性定义，如果一条轴线排不下，如何换行
  - `nowrap`（默认）：不换行
  - `wrap`：换行，第一行在上方
  - `wrap-reverse`：换行，第一行在下方
- flex-flow
  - flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap
- justify-content
  - 项目在主轴上的对齐方式
  - `flex-start`（默认值）：左对齐
  - `flex-end`：右对齐
  - `center`： 居中
  - `space-between`：两端对齐，项目之间的间隔都相等
  - `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍
- align-items
  - 项目在交叉轴上如何对齐
  - `flex-start`：交叉轴的起点对齐
  - `flex-end`：交叉轴的终点对齐
  - `center`：交叉轴的中点对齐
  - `baseline`: 项目的第一行文字的基线对齐
  - `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度
- align-content
  - 定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
  - `flex-start`：与交叉轴的起点对齐
  - `flex-end`：与交叉轴的终点对齐
  - `center`：与交叉轴的中点对齐
  - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布
  - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
  - `stretch`（默认值）：轴线占满整个交叉轴

**子项目的属性**

- `order`
  - 项目的排列顺序。数值越小，排列越靠前，默认为0
- `flex-grow`
  - 项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大
- `flex-shrink`
  - 项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
  - 如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小
- `flex-basis`
  - 定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小
  - 可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间
- `flex`
  - `flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选
- `align-self`
  - 允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性

### Grid 布局

二维布局

## 浮动 float

通过浮动图片，可以实现文字环绕图片的效果

对于左右布局之类可以用flex布局等来实现，避免清除浮动等问题

## 绝对定位

position属性

| 值       | 含义                                                   |
| -------- | ------------------------------------------------------ |
| static   | 默认值，非定位元素                                     |
| relative | 相对自身原本位置偏移，不脱离文档流                     |
| absolute | 绝对定位，相对于最近的非static祖先元素定位，脱离常规流 |
| fixed    | 相对于视口绝对定位                                     |
| sticky   | 粘性定位                                               |



# 常见样式

## 水平和垂直居中

HTML代码

```html
<div class="box">
  <div class="son"></div>
</div>
```

1. 绝对定位+margin反向偏移

   ```css
   .box {
     width: 400px;
     height: 400px;
     position: relative;
   }
   .son {
     width: 200px;
     height: 200px;
     position: absolute;
     left: 50%;
     top: 50%;
     margin-left: -100px;
     margin-top: -100px;
   }
   ```

2. 绝对定位+transform反向偏移（常用）

   优点：不需要知道子元素的width和height

   ```css
   .box {
     width: 400px;
     height: 400px;
     position: relative;
   }
   .son {
     width: 200px;
     height: 200px;
     position: absolute;
     left: 50%;
     top: 50%;
     transform:translate(-50%, -50%);
   }
   ```

3. 子元素绝对定位，所有定位为0，margin设置auto自适应

   ```css
   .box{
     width: 400px;
     height: 400px;
     position: relative;
   }
   .son{
     width: 200px;
     height: 200px;
     position: absolute;
     left: 0;
     top: 0;
     right: 0;
     bottom: 0;
     margin: auto;
   }
   ```

4. 父元素flex布局（弹性布局）

   缺点：某些低版本浏览器不支持

   ```css
   .box{
     width: 400px;
     height: 400px;
     display: flex;
     justify-content: center;
     align-items: center;
   }
   ```

5. display: table

   缺点是html部分增加了一层table-cell层来实现垂直居中

   ```html
   <div class="box">
     <div class="box2">
       <div class="son"></div>
     </div>
   </div>
   ```

   css部分

   ```css
   .box {
     display: table;
   }
   .box2 {
     display: table-cell;
     vertical-align: middle;
   }
   .son {
     margin: auto;
   }
   ```

# CSS优化

## CSS选择器匹配规则

> The style system matches a rule by starting with the rightmost selector and moving to the left through the ruleâ€™s selectors. As long as your little subtree continues to check out, the style system will continue moving to the left until it either matches the rule or bails out because of a mismatch.

CSS样式通过从最右边的选择器开始并通过规则的选择器向左移动来匹配规则。样式系统从最右边的选择器开始持续向左移动，直到它匹配规则或因为不匹配而退出。

> For example, `DIV DIV DIV P A.class0007 {}`. This selector has five levels of descendent matching that must be performed. This sounds complex. But when we look at the rightmost selector, `A.class0007`, we realize that there’s only one element in the entire page that the browser has to match against.

例如，`DIV DIV DIV P A.class0007 {}`. 该选择器有五个必须执行的后代匹配级别。这听起来很复杂。但是当我们查看最右边的选择器时`A.class0007`，我们意识到整个页面中只有一个元素需要浏览器匹配。

> The key to optimizing CSS selectors is to focus on the rightmost selector, also called the *key selector* (coincidence?). Here’s a much more expensive selector: `A.class0007 * {}`. Although this selector might look simpler, it’s more expensive for the browser to match. Because the browser moves right to left, it starts by checking all the elements that match the key selector, “`*`“. This means the browser must try to match this selector against all elements in the page.

优化 CSS 选择器的关键是关注最右边的选择器，也称为*关键选择器*（巧合？）。这是一个更昂贵的选择器：`A.class0007 * {}`. 虽然这个选择器可能看起来更简单，但浏览器匹配它的代价更高。因为浏览器从右向左移动，所以它首先检查与键选择器“ `*`”匹配的所有元素。这意味着浏览器必须尝试将此选择器与页面中的所有元素相匹配。

> The key is focusing on CSS selectors with a wide-matching key selector. 

重点需要关注最右边的关键选择器（key selector）。

参考文章：https://www.stevesouders.com/blog/2009/06/18/simplifying-css-selectors/

## ID选择器的弊端

1. 在一个网页中，不允许存在两个相同的id，一段基于ID选择器的CSS代码只能对一个元素起作用，使得该元素无法重用。

2. 根据CSS选择器的权重值计算规则，ID选择器的优先级高于CLASS选择器（ID选择器权重值为100，CLASS选择器为10），这会导致一种情况：无法直接用CLASS选择器对前面ID选择器中的样式进行覆盖。为了覆盖ID选择器中的样式，需要给CLASS选择器增加类似`!important`等字段以提高优先级，这不利于维护。