## Sass

### 变量声明与使用

```css
// 全局
$nav-color: #F90;
nav {
  // 局部
  $width: 100px;
  width: $width;
  color: $nav-color;
}

//编译后

nav {
  width: 100px;
  color: #F90;
}
```

变量中引用变量

```css
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}
```

### 嵌套CSS规则

#### 嵌套规则

```css
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
```

```css
 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

#### 父选择器标识符&

sass解开嵌套规则时会使用空格连接，在处理:hover伪类时会出错

```css
article a {
  color: blue;
  &:hover { color: red }
}
```

#### 群组选择器的嵌套

```css
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
```

#### 子选择器>、同层选择器+、~

可以写在外层选择器后面、或者里层的前面

```css
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
```

#### 嵌套属性

```css
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
```

### 导入SASS文件

CSS只有在执行到@import是才会去下载其他的CSS文件

SASS的@import会在构建时引入

- 使用SASS的部分文件

约定SASS局部文件的文件名以下划线开头，编译的时候不会生成单独的css文件

```
@import "themes/night-sky"; //导入时省略下划线
```

- 默认变量值 !default
- 嵌套导入，可以在css规则内使用@import

### 静默注释（没啥用）

使用//注释的内容，不会生成在css中，也没啥用，构建工具会进行处理

```css
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

### 混合器

混合器使用@mixin，用于重用**大段的代码**，使用@include引入

```css
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}

notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners; //引用
}
```

#### 混合器传参

```css
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

/* 按顺序 */
a {
  @include link-colors(blue, red, green);
}

/* 也可指定参数名 */
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}

/* SASS最终生成 */
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```

#### 默认参数值

```css
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

### 选择器继承(慎用)

通过@extend语法，会继承该选择器命中的元素所应用的样式

```css
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

不要在CSS规则中使用后代选择器去继承规则，会造成不确定性，增加编译后的代码量

```css
.foo .bar { @extend .baz; } //不要使用这种方式
.bip .baz { a: b; }
```

### SassScript（补充）

引入了数学计算功能

## Less

Less学习较为容易

### 变量

```css
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

### 混合

混合可以使用类.class或者#id进行定义

```css
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered(); //混合
}

.post a {
  color: red;
  .bordered(); //混合
}
```

#### 对混合进行分组

```css
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}

#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```

#### 对混合进行映射

Less3.5版本后，可以将混合和规则集作为键值对使用

```css
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```

### 嵌套

和sass基本一样,`&` 表示当前选择器的父级

```css
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}

```

#### @嵌套规则和冒泡

@规则可以与选择器以相同的方式嵌套，@规则放在前面，其他的相对位置保持不变

也就是@规则冒泡到最前面

```css
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
```

编译后

```css
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```

### 运算

算术运算符+、-、*、/可以对数字、颜色、变量进行运算

### 转义

这个功能是Sass没有的，可以使用任意字符串作为属性或者变量值

```css
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}

// less3.5之后可以简写为
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```

### 函数(补充)

Less内置了多种函数用于转换颜色、处理字符串、算术运算

```css
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```

命名空间

## stylus



## PostCSS

