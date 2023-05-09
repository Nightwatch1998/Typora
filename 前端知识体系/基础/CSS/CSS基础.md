## CSS基础

### 背景

background背景简写顺序

```css
background-color /*背景色*/
background-image /*图片背景*/
background-repeat /*是否重复*/
background-attachment	/*是否附着*/
background-position	/*背景图片位置*/
```

### 边框

- 每条边的样式可以单独设定

```css
border-style /*边框样式，是必须的*/
border-width /*设置每条边的宽度*/
border-color /*设置每条边的颜色*/
border-top-style: dotted; /*指定每个边框的样式*/
```

- 简写属性

```css
border: 5px solid red;
```

- 简写单条边属性

```css
border-left: 6px solid red;
```

- 圆角边框

```css
border-radius: 5px;
```

### 高度/宽度

height和width可设置为一下值：

- auto：浏览器默认计算
- length：以px、cm等计算
- %：百分比
- initial：默认值
- inherit：从父元素继承

**max-width**: 设置元素的最大宽度，内用区域大于该宽度是，会自动收缩

**min-width**：设置元素最小宽度，内容区域小于该宽度时，会自动扩展

### 轮廓

轮廓实在元素周围绘制一条线，在边框之外，**以凸显元素**

```css
ouline
outline-style
outline-color
/*thin 1px,medium 3px,thick 5px*/
outline-width:thin; 
/*轮廓偏移*/
outline-offset
```

注：outline属性不能为每条边单独设置

### 文本(重要)

- 文本颜色，color属性

#### 文本对齐

- 文本水平对齐

```css
text-align:left; /*水平对齐方式*/
text-align:justify; /*拉伸每一行，使得每一行具有相同宽度*/
```

- 文本垂直对齐

```css
vertical-align:top;
```

- 文本方向

```css
direction
unicode-bidi
```

- 文本装饰，text-decoration下划线、上划线、删除线

- 文本大小写转换text-transform

#### 文字间距

```css
/*文本缩进*/
text-indent:50px;
/*字符之间的间距*/
letter-spacing: 3px;
/*行高*/
line-height:0.8;
/*单词间距*/
word-spacing：10px;
/*空白*/
white-space：nowrap;
/*文本阴影效果，水平，垂直*/
text-shadow:2px 2px;
```

### 字体

#### 5个通用字体族

- 衬线字体（Serif）- 在每个字母的边缘都有一个小的笔触。它们营造出一种形式感和优雅感。
- 无衬线字体（Sans-serif）- 字体线条简洁（没有小笔画）。它们营造出现代而简约的外观。
- 等宽字体（Monospace）- 这里所有字母都有相同的固定宽度。它们创造出机械式的外观。
- 草书字体（Cursive）- 模仿了人类的笔迹。
- 幻想字体（Fantasy）- 是装饰性/俏皮的字体。

#### 字体配置

```css
/*以需要的字体开始，以通用的字体结束*/
font-family: "Times New Roman", Times, serif;
/*指定斜体*/
font-style: italic;
/*字体粗细*/
font-weight: bold;
```

#### 字体大小

```css
/*像素设置字体大小*/
font-size:40px;
/*使用em设置,1em默认为16px*/
font-size：1em;
/*响应式单位vm,1vm为视口宽度的1%*/
font-size:10vw;
```

#### 简写属性font

```css
font-style
font-variant
font-weight
font-size/line-height 必须
font-family 必须
```

### 图标

使用图标库，通过CSS设置

### 链接

四种链接状态：

- a:link,未访问的链接
- a:visited,用户访问过的链接
- a:hover,用户鼠标悬停时
- a:active：链接被点击时

### 列表

在使用简写属性时，属性值的顺序为：

- `list-style-type`（如果指定了 list-style-image，那么在由于某种原因而无法显示图像时，会显示这个属性的值）
- `list-style-position`（指定列表项标记应显示在内容流的内部还是外部）
- `list-style-image`（将图像指定为列表项标记）

### 表格

```css
/*表格边框*/
border: 1px solid black;
/*合并表格边框*/
border-collapse: collapse;
/*表格对齐*/
text-align: center;
vertical-align: bottom;
```

水平分割线

```css
border-bottom: 1px solid #ddd;
```

条状表格

```css
tr:nth-child(even) {background-color: #f2f2f2;}
```

