## 引用类型

### RegExp类型



#### 实例方法

- reg.exec(text) 返回捕获组信息
- reg.test(text) 返回true/false

#### 构造函数的属性

​	类似于类的静态属性，适用于作用域的所有正则表达式，并且基于所执行最后一次正则操作而变化。可以通过长属性名和短属性名两种方式访问。

| input     | $_   | 最近一次要匹配的字符串 |
| --------- | ---- | ---------------------- |
| lastMatch | $&   | 最近一次匹配项         |
| lastParen | $+   | 最近一次匹配的捕获组   |



### Function类型

#### 函数的属性

- 函数是对象，函数名是指针
- 函数声明会有声明提升

- 函数内部属性arguments和this
- 函数对象的属性与方法:length表示命名参数的个数、prototype

#### 扩充作用域

- apply方法接收两个参数，一个是运行在其中的作用域，另一个是参数数组
- call方法的区别在于参数要逐个列举出来
- bind方法会创建一个函数实例，this值会被绑定到传给bind的值

#### 基本的包装类型

引用类型的实例在执行流离开当前作用域之前一直都保存在内存中

自动创建的基本包装类型对象只存在于代码执行的一瞬间

- Boolean
- Number：toString(radix)指定进制、toFixed()指定位数、toExponential()返回e表示法
- String：slice()、substr()、substring()、indexOf()、lastIndexOf()、trim()方法去除字符串前后空格

#### 单体内置对象

Global:不属于其他任何对象的属性和方法，都是它的属性和方法

- URI编码方法，编码与解码
- eval()方法
- JS中window对象扮演了Global对象的角色

Math对象

## 面向对象的程序设计

### 理解对象

#### 属性类型

数据属性：

- [[Configurable]]:能否通过delete删除属性、能否修改特性、设置为false后不能再设置其他的
- [[Enumerable]]:能否通过for-in循环返回属性
- [[Writable]]:能否修改属性的值
- [[Value]]:属性的数据值

​	Object.defineProperty()可以修改属性默认的特性

访问器属性(除可配置和可迭代之外)：

- [[Get]]
- [[Set]]

​	Object.defineProperties()一次性定义多个属性

### 创建对象

- 工厂模式：无法解决对象识别的问题

- 构造函数模式：创建对象、将作用域赋给新对象(this指向了这个新对象)、执行构造函数中的代码、返回新对象。函数属性需要定义在外部，内部绑定在this上，不然无法共享

- 原型模式：prototype属性。所有对象实例共享属性和方法。实例的指针仅仅指向原型而不是构造函数

  - obj.prototype.constructor指向自身
  - 可以通过isPrototypeOf()来判断一个对象是否为另一个对象的原型
  - hasOwnProperty()可以判断属性在实例中还是原型中，指定了实例属性后
  - getOwnPropertyNames()得到所有的实例属性

  缺点：引用类型也会共享

- **组合使用构造函数与原型模式：构造函数定义实例属性，原型定义方法和共享的属性。**
- 寄生对象构造函数模式：可用于扩展基本对象的功能

### 继承

- 原型链：引用类型参数会被共享；不能向超类型的构造函数中传递参数
- 借用构造函数：call()绑定子类的作用域。
  - 传递参数
  - 存在的问题：方法定义在构造函数中，无法复用
- 组合继承：原型链继承原型属性和方法、借用构造函数继承实例属性；缺点是会调用两次超类型构造函数
- 原型式继承：内部封装构造函数，指定原型，并返回实例，Object.create()
- 寄生式继承：创建一个用于封装继承过程的函数
- **寄生组合式：不必为了指定子类型的原型而调用超类型的构造函数**

## 函数表达式

函数声明的重要特征是声明提升

### 递归

```js
function factorial(num){
    if(num<=1){
        return 1
    }else{
        return num*factorial(num-1)
    }
}
var anotherFactorial = factorial
factorial = null
console.log(anotherFactorial(4))//出错
```

这种情况会出错,解决办法：

```
function factorial(num){
    if(num<=1){
        return 1
    }else{
        return num*arguments.callee(num-1)
    }
}
```

### 闭包

闭包是指有权访问另一个函数作用域中的变量的函数

闭包可以通过公有方法访问在包含作用域中定义的对象，实现”私有对象“

有权访问私有变量的公有方法叫做特权方法

### 模仿块级作用域

在多次声明同一个变量时，JS会对后续的声明视而不见

```
(function(){
	//这里是块级作用域
})();
```

将函数声明放在圆括号中，紧随其后立即调用这个函数，可以使用私有作用域，比如for循环的i

### 私有变量

- 构造函数中通过闭包对私有变量的访问进行封装，每个实例会共享私有变量
- 模块模式通过为单例添加私有变量和特权方法使其得到增强，初始化+私有变量
- 增强的模块模式，返回对象前加入其增强的代码

## BOM

### window对象

window既是JS访问浏览器窗口的接口，优势ECMAScript规定的global对象。

- **全局作用域**声明的变量、函数会成为window对象的属性和方法
- **框架**存储在frames集合中，每个框架都拥有自己的window对象
  - top对象指向最外层的**框架**
  - parent指向父级框架
- 窗口位置

```
screenLeft,screenTop，(IE、Safari、Opera、Chrome)相对于屏幕左边和右边的位置
ScreenX,ScreenY,(Firefox、Safari、Chrome)
```

- 窗口大小

```
outerWidth、outerHeight返回浏览器窗口本身的值
innerWidth、innerHeight返回页面视图区的值
document.doucmentElement.client(Left|Height|Width|Top)返回页面视口信息，(兼容IE)
```

- 导航和打开窗口

```
window.open()
```

- 系统对话框

```
alert() //弹出信息
confirm() // 确认窗口
prompt() // 提示窗口
```

### location对象

location对象保存了当前窗口加载的文档的信息

- hash：URL中的hash
- host：服务器名称+端口号
- hostname：不带端口号的服务器名称
- href：返回当前加载页面的完整URL
- port：端口号
- protocol：协议

改变浏览器位置：

```
location.assgin()
window.location = ""
location.href = "" //最常用
location.hash = "" //修改属性
```

### navigator对象*

用于检测显示网页的浏览器类型

- 检测插件：plugins

### screen对象*

用于表明客户端显示器的能力，大多数属性为只读

### history对象

history对象保存用户上网的历史记录

```js
history.go(-1) // 后退一页
history.go(1) // 前进一页
history.forward()
history.back()
```

## 客户端检测

### 能力检测

```js
// 检测属性是否存在
if(object.propertyInQuestion){
	// 使用object.propertyInQuestion
}
```

```
// 检测对象是否可排序
function isSortable(object){
	return typeof object.sort == "function"
}
```

### 怪癖检测

浏览器的一些BUG

### 用户代理检测

#### userAgent字符串的历史

- 早期的浏览器 ：语言、平台、加密类型
- Netscape Navigator3和Internet Explorer3,Mozilla/2.0
- NetScape Communicator 4和IE4~IE8，Mozilla/4.0
- **Geoko是FireFox的呈现引擎**
- **Webkit是Safari的呈现引擎**
- Chrome浏览器以WebKit作为呈现引擎
- linux下的Konqueror，使用KHTML开源呈现引擎
- Opera正确地标识了自身及其版本号
- IOS和Android的默认浏览器都基于Webkit

#### userAgent检测

- 识别呈现引擎：ie、gecko、webkit、khtml
- 识别浏览器：ie、firefox、safari、chrome、opera
- 识别平台：Windows、Mac和Unix
- 识别windows操作系统版本号
- 识别移动设备
- 识别游戏系统

## DOM

### DOM1

1998年10月DOM1级规范成为W3C标准

9种节点层次

DOM操作技术：

- 动态脚本
- 动态样式
- 创建表格
- 使用NodeList

### DOM扩展

- jQuery的核心就是通过CSS选择符查询DOM文档取得元素的引用
- 元素遍历
- H5定义了大量的JS API
  - 焦点管理
  - 设置字符集
  - 滚动页面

### DOM2和DOM3

扩展DOM API，满足操作XML的所有需求
