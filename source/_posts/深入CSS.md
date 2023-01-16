---
title: 深入CSS
date: 2023-01-06 15:47:02
tags:
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

## CSS工作机制

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