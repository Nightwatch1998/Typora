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

**浏览器窗口变化时**：

**max-width**: 设置元素的最大宽度，内容区域大于该宽度时，会自动收缩，改善对小窗口的处理

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
font-size: 40px;
/*使用em设置,1em默认为16px*/
font-size：1em;
/*响应式单位vm,1vm为视口宽度的1%*/
font-size: 10vw;
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

## CSS中级教程

### 元素排版

#### display布局方式

**display属性**

| 值           | 描述                 |
| :----------- | -------------------- |
| none         | 此元素不显示         |
| block        | 显示为块级元素       |
| inline       | 默认，显示为内联元素 |
| inline-block | 行内块               |
| table        | 块级表格             |
| inherit      | 继承父元素的属性     |

**visibility属性**

| 值       | 描述                       |
| -------- | -------------------------- |
| visible  | 默认可见                   |
| hidden   | 不可见                     |
| collapse | 表格中使用可删除一行或一列 |
| inherit  | 继承父元素                 |

#### position定位

- static 正常文档流
- relative 相对于元素自身位置
- fixed 相对于视口
- absolute 绝对定位，相对于最近的定位祖先元素
- sticky 根据用户的滚动位置进行定位

#### overflow溢出

overflow属性：

- visible 溢出不会剪裁，内容会在元素框外渲染
- hidden 溢出被裁减，其余内容不可见
- scroll 溢出被裁减，其余内容滚动可见
- auto 溢出的时候添加滚动条

overflow-x 控制水平

overflow-y 控制垂直

#### float浮动和清除

float ： left|right|none

clear：

- none 允许两侧都有浮动元素
- left 左侧不允许浮动元素
- right 右侧不允许浮动元素
- both 左右均不允许浮动元素

设置clear的元素会在下方显示

如果一个元素比包含它的元素高，并且是浮动的，它将溢出到其容器外

- 使用overflow:auto解决
- clearfix 添加伪元素解决

```css
.clearfix::after {
  content: "";
  clear: both;
  display: table;
}
```

#### box-sizing

允许在元素的总宽度和高度中**包括内边距和边框**

```css
box-sizing: border-box;
```

#### display:inline-block

允许元素设置宽高

#### 对齐

- 居中对齐元素：margin:auto；需要设置width
- 居中对齐文本：text-align:center；需要设置width
- 居中对齐图像

```css
img {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 40%;
}
```

- 左右对齐position

```css
.right {
  position: absolute;
  right: 0px;
  width: 300px;
}
```

- 左右对齐float

```css
.right {
  float: right;
  width: 300px;
}
```

- 垂直对齐padding
- 垂直对齐line-height=height
- 垂直对齐position和transform

```css
.center { 
  height: 200px;
  position: relative;
  border: 3px solid green; 
}

.center p {
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

- 垂直对齐使用flex

```css
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
  border: 3px solid green; 
}
```

**总结：**

- **水平对齐：margin、text-align、float、position**
- **垂直对齐：padding、line-height、position、flex**

### 选择器

#### 组合器

- 后代选择器（空格）
- 子选择器（>）
- 相邻兄弟选择器（+）
- 通用兄弟选择器（~）

#### 伪类：

伪类用于定义元素的**特殊状态**

| 选择器        | 例子           | 例子描述                        |
| ------------- | -------------- | ------------------------------- |
| :active       | a:active       | 选择活动的链接                  |
| :link         | a:link         | 选择未访问的链接                |
| :visited      | a:visited      | 选择所有已访问的链接            |
| :hover        | a:hover        | 选择鼠标悬停时的链接            |
| :focus        | input:focus    | 获得焦点的input元素             |
| :first-child  | p:first-child  | 选择作为父元素第一个元素的p元素 |
| :nth-child(2) | p:nth-child(2) | 选择作为父元素第二个元素的p元素 |
| :valid        | input:valid    | 选择所有具有有效值的input元素   |

#### 伪元素：：

伪元素用于设置元素**指定部分**的样式

| 选择器         | 例子            | 例子描述                   |
| -------------- | --------------- | -------------------------- |
| ::after        | h1::after       | h1元素后添加内容           |
| ::before       | h1::before      | h1元素前添加内容           |
| ::first-letter | p::fisrt-letter | 向文本的首字母添加特殊样式 |
| ::first-line   | p::first-line   | 为所有p元素的首行添加样式  |
| ::selection    | ::selection     | 匹配用户选择的元素         |

### 常用组件原理

#### 导航栏

导航栏就是链接列表

页面

```html
<ul>
  <li><a href="index.asp">Home</a></li>
  <li><a href="news.asp">News</a></li>
  <li><a href="contact.asp">Contact</a></li>
  <li><a href="about.asp">About</a></li>
</ul>
```

- 垂直导航栏

```css
/*从列表删除项目符号以及内外边距*/
ul {
    list-style-type:none;
    margin: 0;
    padding: 0;
    width: 200px;
  	background-color: #f1f1f1;
}
/*垂直导航栏*/
li a {
    display:block; /*整个链接区域都可以被单击*/
    width: 60px;
    padding:8px 16px;
    text-decoration: none;
}
/*鼠标悬停改变颜色*/
li a:hover{
    background-color:#555;
    color:white;
}
```

- 水平导航栏：行内或者浮动

```css
li {
    display:inline;
}
```

#### 下拉菜单

通过hover和display实现

#### 计数器

用于多集目录编号

#### 图片库

图像精灵

#### Tooltip

- 绝对定位
- visible
- 伪元素实现箭头

#### 按钮

#### 分页器

### css特异性

**特异性计算方法：**

- **内联 1000**
- **ID 100**
- **属性、类、伪类 10**
- **元素名称、伪类 1**

特异性规则：

- 特异性相同，应用**最新**的规则
- **ID选择器**比属性选择器特异性更高
- **类选择器**比元素选择器特异性更高
- style标签比外部样式表更具体

## CSS3高级教程

### 美化效果

#### border-radius圆角

border-radius可以接收一到四个值：

- 四个值：左上、右上、右下、左下
- 三个值：左上、（右上、左下）、右下
- 两个值：（左上、右下）、（右上、左下）
- 一个值：四个角

椭圆角：

```
border-radius: 50px / 15px;
```

#### border-image边框图像

属性有五部分：

- 用作边框的图像
- 边框向内偏移
- 图像边框的宽度，超过50的话，边线就消失了
- 定义中间部分重复还是拉伸

```css
border-image: source slice width outset repeat|initial|inherit;
```

#### 背景

- background-clip 背景绘制区域(其他区域不显示)
- background-origin 背景图像放置位置



##### 多重背景

background-image添加多个背景，不同背景用逗号分开，第一幅图像最靠近观察者

```css
#example1 {
  background-image: url(/i/photo/flower.gif), url(/i/paper.jpg);
  background-position: right bottom, left top;
  background-repeat: no-repeat, repeat;
  padding: 15px;
}
```

也可以通过background简写

##### 背景尺寸

- 长度、百分比
- contain 宽高<=内容区域
- cover 宽高>=内容区域

#### 渐变

两种渐变类型：

- 线性渐变（向下/向上/向左/向右/对角线）,**默认从上到下**

```css
/*语法*/
background-image: linear-gradient(direction, color-stop1, color-stop2, ...);

background-image: linear-gradient(to bottom right, red, yellow); /*向右下*/ background-image: linear-gradient(to right, red, yellow);/*向右*/ 
background-image: linear-gradient(45deg,red,yellow);/*使用角度*/
background-image: linear-gradient(to right,rgba(255,0,0,0),rgba(255,0,0,1)); /*使用透明度*/
```

- 径向渐变（由其中心定义），默认shape为椭圆，size为最远角，position为中心
  - shape：circle、ellipse
  - size：渐变的大小，**即渐变到哪里停止**

```css
/*语法*/
background-image: radial-gradient(shape size at position, start-color, ..., last-color);
background-image: radial-gradient(red, yellow, green);/*从中心开始*/
background-image: radial-gradient(circle, red, yellow, green); /*圆形渐变*/
```

#### 阴影

在文本和元素上添加阴影

- text-shadow

```css
text-shadow:2px 2px; //指定水平阴影和垂直阴影
text-shadow:2px 2px red; //添加颜色
text-shadow:2px 2px 3px red; //添加模糊
```

- box-shadow类似

#### 文本效果

- text-overflow 文字溢出，裁减或省略

```
text-overflow：clip;
text-overflow: ellpsis;
```

- word-wrap 长文字被折断并换行到下一行

```css
word-wrap: break-word;
```

- word-break 指定换行规则

```css
word-break: keep-all; //连字符处打断
word-break: break-all; //任何位置打断
```

#### 网络字体

在@font-face定义字体

- TrueType（TTF）windows、mac操作系统常用字体
- OpenType (OTF)  可缩放
- WOFF 2009年开发，是W3C推荐标准，本质是压缩OTF、TTF并添加元数据
- WOFF 2.0 提供更好的压缩

```css
@font-face {
  font-family: myFirstFont;
  src: url(sansation_light.woff);
}

div {
  font-family: myFirstFont;
}
```

### CSS动画

#### 2D转换 

CSS transform属性，允许移动、旋转、缩放、倾斜元素

```css
transform: translate(50px, 100px);// 向右移动50px，向下移动100px
transform: rotate(20deg); // 顺时针旋转20度
transform: rotate(-20deg); // 逆时针旋转20度

transform: scale(2, 3); //宽变为2倍，高变为3倍
transform: scaleX(2); //宽变为2倍
transform: scaleY(0.5); //高变为0.5倍

transform: skew(20deg,10deg) //沿着x轴，y轴倾斜
transform: skewX(20deg); // 沿着x轴倾斜

// x缩放、y倾斜、x倾斜、y缩放、x平移、y平移
transform: matrix(1, -0.3, 0, 1, 0, 0);
```

#### 3D转换

3D转换方法

```css
transform: rotateX(150deg); //绕x轴旋转150度
transform: rotateY(130deg); //绕y轴旋转
transform: rotateZ(130deg); //绕z轴旋转
```

3D变换属性：

- transform-origin 改变元素位置
- transform-style 子元素在3D空间的显示
- perspective：**观察者与z=0平面的距离**，**值越小，视角越深**，子元素获得透视效果
- perspective-origin：x-positon y-position；**3D元素的底部位置**,透视点在该元素的位置

#### 过渡（属性变化）

首先确定添加效果的**CSS属性**，以及效果的**持续时间**

- transition：属性和持续时间
- transition-property：**要添加过渡效果的属性**
- transition-duration：**持续时间**

- transition-delay：过渡效果的**延迟**（s）

```css
transition: width 2s
transition: width 2s, height 2s, transform 2s; // 过渡+转换
transition: width 2s linear 1s; // 简写：属性、持续时间、函数、延迟
```

- transition-timing-function：过渡时间函数
  - ease 缓慢开始，先加速，后减速
  - ease-in 缓慢开始
  - ease-out 缓慢结束
  - ease-in-out 开始和结束较慢
  - linear 线性
  - cubic-bezier 自定义

#### 动画（样式变化）

- @keyframes 定义关键帧样式
- animation-name 动画名称，需要绑定在某个元素上
- animation-duration 持续时间
- animation-delay 延迟
- animation-iteration-count 动画运行次数，infinite无限循环
- animation-direction  动画向前播放、向后播放、交替播放
- animation-timing-function 和过渡中的速度曲线类似
- animation-fill-mode  不播放动画时元素的样式
- animation 属性简写

实例

```css
/* 动画代码 */
@keyframes example {
  from {background-color: red;}
  to {background-color: yellow;}
}

/* 向此元素应用动画效果 */
div {
  width: 100px;
  height: 100px;
  background-color: red;
  animation-name: example;
  animation-duration: 4s;
}
```

### 多列
