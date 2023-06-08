## WebPack

作用是什么：

- 模块打包：将不同的模块文件打包整合
- 编译兼容：通过webpack的loader机制，将.vue .less .jxs等编译为浏览器可识别的文件
- 能力扩展：插件机制，按需加载，代码压缩等等

### 模块打包的原理?

1. 读取webpack配置参数
2. 启动webpack，创建Compiler对象开始解析项目
3. 从**入口文件entry**开始解析，找到其导入的依赖模块，递归遍历分析，形成**依赖关系树**
4. 对不同文件类型的依赖模块文件使用相应的**Loader**进行编译，转为Js，css文件
5. 整个过程webpack通过发布订阅模式，向外抛出一些hooks，而**plugins**可通过监听这些关键的事件节点，执行插件任务从而达到干预输出结果的目的

最终打包出来的bundle文件是一个IIFE的执行函数

三个变量一个方法;

- **webpack_modules** 存放了编译后的各个文件模块的JS内容
- __webpack_module_cache__  用来做模块缓存
- __webpack_require__   Webpack内部实现的一套依赖引入函数

### sourceMap

sourceMap是一项将编译、打包、压缩后的代码映射回源代码的技术，可以帮助开发者快速定位到源代码的位置，提高开发效率。

.map文件的格式

```js
{
  "version" : 3,                          // Source Map版本
  "file": "out.js",                       // 输出文件（可选）
  "sourceRoot": "",                       // 源文件根目录（可选）
  "sources": ["foo.js", "bar.js"],        // 源文件列表
  "sourcesContent": [null, null],         // 源内容列表（可选，和源文件列表顺序一致）
  "names": ["src", "maps", "are", "fun"], // mappings使用的符号名称列表
  "mappings": "A,AAAB;;ABCDE;"            // 带有编码映射数据的字符串
}
```

map文件只要不打开开发者工具，是不会加载的。

线上环境一般有三种处理方案：

- 借助第三方监控平台
- 只显示具体行数和源代码错误栈
- 通过nginx将.map只对白名单开放

### Loader的实现思路？

webpack默认只能打包js代码，当存在非js代码时，需要先进性必要的转换。

Loader支持以数组形式配置，webpack会按顺序链式调用没给loader。因此Loader的返回值必须是JS代码字符串

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

常见的loader：

- css-loader：允许require css
- less-loader
- sass-loader
- babel-loader：将ES6转换为ES
- file-loade：分发文件到output目录

### Plugins实现思路？

webpack基于发布订阅模式，在运行的生命周期会广播出许多事件，插件监听这些事件，就可以在特定的阶段执行自己的插件任务。实现**打包优化、资源管理、环境变量注入**等，本质是一个具有apply方法的js对象

生命周期钩子:comiler-hooks

依赖钩子:compilation hooks

常用的插件：

- HtmlWebpackPlugin：打包结束自动生成一个html并引入js
- clean-webpack-plugin：删除构建目录
- mini-css-extract-plugin：提取css到单独的文件

### 热更新原理

热更新HMR，又称为热替换，不用刷新浏览器而将新变更的模块替换旧的模块

WDS与浏览器之间维护了一个Websocket，本地资源变化时，WDS会向浏览器推送更新，并带上构建时的hash，

客户端进行diff，找到差异后，向WDS发起Ajax请求，来获取更改内容。

```js
const webpack = require('webpack')
module.exports = {
  // ...
  devServer: {
    // 开启 HMR 特性
    hot: true
    // hotOnly: true
  }
}
```

总结：

- 通过webpack-dev-server 创建两个服务器提供静态资源服务**express和socket服务**
- express server负责直接提供静态资源服务
- socket server是一个webocket长连接，双方可以通信
- socket server监听到对应的模块发生变化时，会生成两个文件.json 和.js
- 通过长连接，发送这两个文件到客户端
- 客户端通过HMR机制，加载这两个文件

### tree shaking原理

ES6 Module引入进行静态分析，故而编译的时候正确判断到底加载了哪些模块

静态分析流，判断哪些模块和变量未被使用或引用，进而删除对应代码。

### webpack proxy工作原理？为什么能解决跨域？

接收客户端请求后转发给其他服务器，利用http-proxy-middleware实现请求转发

### 文件指纹是什么？

打包后输出的文件名的后缀

- hash：和整个项目的构建有关
- ChunkHash：不同的entry，不同的chunkhash
- Contenthash：根据文件内容定义

### 提高webpack构建速度

- 优化loader配置
- 合理使用resolve.extensions，resolve.modules，resolve.alias
- 合理使用sourceMap粒度

### webpack前端优化

- js代码压缩：terser压缩代码
- css压缩：去除无用空格
- 文件压缩
- tree shaking：usedExports：true，
- 代码分离

### 其他构建工具

rollup

优点：

- 代码效率简洁、效率更高
- 默认支持Tree-shaking

缺点：

- 加载其他类型资源，导入CommonJS模块，都需要插件完成
- 不支持HMR
- 需要使用第三方模块

parcel

自动安装依赖，支持热替换

vite

- 快速冷启动
- 即时的模块热更新
- 真正的按需编译

## 模块化

模块化的好处：

- 避免命名冲突
- 更好的分离，按需加载
- 更高复用性
- 高可维护性

模块化的演进：

- 全局function：污染全局命名空间，看不出模块的关联
- 简单对象封装：数据不安全
- IIFE匿名函数自调用：数据私有，暴露方法。存在模块依赖问题
- IIFE模式增强：**引入依赖**，

### CommonJS

CommonJS最初是为Nodejs环境设计的，也被广泛用于浏览器端的模块化开发。每个文件一个模块，有自己的作用域。文件里定义的变量，函数，类都是私有的。对其他文件不可见。

它有四个重要的环境变量为模块化提供支持：`module、exports、require、global`

CommonJS用同步的方式加载模块。在服务端，模块存在本地磁盘，读取很快。浏览器端应该使用异步加载。

- 使用module.exports

```js
// 定义math.js
var basicNum = 0;

function add(a,b){
    return a + b
}
// module.exports语句只能有一个，后者会覆盖前面的
module.exports = { //这里写需要向外暴露的变量
    add,
    basicNum
}

// 导入
var math = require('./math');
math.add(2,5);

// 也可解构访问
const { add, basicNum } = require('./math')
add(2,5)
```

- exports对象是module.exports的一个引用，可以向exports添加属性和方法

```js
// 导出属性
exports.foo = 'bar';

// 导出方法
exports.add = function(a, b) {
  return a + b;
};

// 解构导入
const { add, foo } = require('./math')
add(1,2)
```

不同的导出方式，导入时是一样的。

不建议混用

### ES6 Module

ES6模块的设计思想是尽量静态化,模块不是对象，使得编译时就能确定模块的依赖关系，同时无法实现条件加载。导入本地模块不能省略.js后缀。

使用ES6模块，需要在package.json配置“type”：“module”

主要由两个命令构成:export和import

- export对象语法：


```js
/** 定义模块 math.js **/
var basicNum = 0;
var add = function (a, b) {
    return a + b;
};
export { basicNum, add };

/** 引用模块 **/
import { basicNum, add } from './math.js';
function test(ele) {
    ele.textContent = add(99 + basicNum);
}
```

- export default语法

```js
//定义输出
export default { basicNum, add };

//引入
import math from './math.js';
function test(ele) {
    ele.textContent = math.add(99 + math.basicNum);
}
```

### AMD与require.js

AMD（Asynchronous Module Definition）规范采用**异步方式**加载模块，加载完立即执行,依赖前置，提前执行。

浏览器端一般采用AMD规范，加载AMD模块需要先引入require.js

define函数来自于require.js

```js
// 定义没有依赖的模块
define(function(){
   return {
       foo: "bar"
   }
})

// 定义有依赖的模块，第一个参数是依赖模块的数组
define(['module1', 'module2'], function(m1, m2){
   return {
       baz: function(){
           // 方法
       }
   }
})

// 加载模块，第一个参数是加载的模块标识符
require(['module1','module2'], function(m1, m2){
    // 使用模块
   console.log(m1,m2)
})
```

### CMD与sea.js

CMD类似于AMD，**依赖就近，但是延迟执行**。可以通过require动态加载依赖项。

define来自sea.js，require.js也可以处理cmd模块

```js
// 定义模块
define(function(require, exports, module) {
  // 通过require函数加载依赖模块
  var dependency1 = require('dependency1');
  var dependency2 = require('dependency2');

  // 模块代码
  // 可以使用依赖模块dependency1和dependency2
  var foo = 'bar';
  var baz = function() {
    // 模块方法
  };

  // 使用exports对象导出模块内容
  exports.foo = foo;
  exports.baz = baz;
});

// 加载模块
require(['myModule'], function(myModule) {
  // 使用导入的模块
  console.log(myModule.foo);  // 输出: 'bar'
  myModule.baz();
});
```

### UMD

UMD（Universal Module Definition）模块是一种通用的模块定义规范，使模块能够在不同的浏览器环境运行，包括浏览器、Nodejs和其他支持模块化的环境。

可以理解为是一种兼容性的模块

```js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD环境
    define(['dependency1', 'dependency2'], factory);
  } else if (typeof exports === 'object' && typeof module === 'object') {
    // CommonJS环境
    var dep1 = require('dependency1');
    var dep2 = require('dependency2');
    module.exports = factory(dep1, dep2);
  } else {
    // 全局变量环境
    root.ModuleName = factory(root.Dependency1, root.Dependency2);
  }
}(this, function (dep1, dep2) {
  // 模块的实现
  return {
    // 导出的模块内容
  };
}));
```



### ES6模块与CommonJS模块的差异

- CommonJS模块输出的是**值的拷贝**，ES6输出的是**引用**
- CommonJS模块是运行时加载，ES6是编译时加载，所以无法条件加载
- CommonJS顶级变量和函数都是**公有的**，ES6顶级变量和**函数私有**
- CommonJS导入本地模块可以省略.js后缀，ES6不可以。

## 性能优化

性能优化分类：

1. 代码层面(运行时优化)

- 防抖和节流

- 减少回流和重绘
- 事件委托
- css，js脚本放在最底部
- 减少DOM操作
- 按需加载

2. 构建方面(加载时优化)

- 压缩代码文件
- 开启gzip压缩
- 常用的第三方库使用CDN服务

### 手动检查

检查加载性能：

- 白屏时间:从输入网址，到页面开始显示内容的时间
- 首屏时间：从输入网址，到页面完全渲染的时间

检查运行性能：

- performance标签，查看高亮重绘区域，显示帧渲染信息

### 利用工具检查

监控工具sentry

chrome工具Lighthouse对网站进行全面的性能和质量分析

## 前端工程化

前端工程化是指利用工具和技术来提高前端开发效率和代码质量的过程。前端工程化的目的是通过自动化和规范化的流程来提高开发效率、减少开发成本、改善代码质量和提高用户体验。

### 统一规范（eslint）

1. 代码规范：

- 规范的代码可以促进团队合作
- 降低维护成本
- 有助于code review
- 养成代码规范的习惯，有助于自身的成长

制定代码规范可以在在好的代码规范上进行修改。

**eslint**可以检查代码是否符合团队制定的规范。

2. git规范：分支管理规范和、git commit 规范
3. 项目规范：项目文件的组织方式个命名方式

```
├─api （接口）
├─assets （静态资源）
├─components （公共组件）
├─styles （公共样式）
├─router （路由）
├─store （vuex 全局数据）
├─utils （工具函数）
└─views （页面）
```



### 构建

通过自动化构建工具来处理复杂的构建任务，

### 模块化开发

模块化的开发方式，以便更好的管理代码和模块的依赖关系

### 测试（jest）

测试代码的正确性和稳定性，找出bug，越早发现bug越好。减轻修改代码时的心智负担。

单元测试：

- 对函数做测试
- 对类做测试
- 对组件做测试

### 部署

持续部署CD

轮询：每隔一段时间就自动执行打包、部署操作

监听webhook事件：监听push事件，当事件触发时，自动执行定义好的脚本

### 监控

chrome开发者工具

1. 性能监控：利用window.performance进行数据采集，timing属性，包含了页面加载各个阶段的起始和结束时间，以及js,css,img的加载时间

2. 错误监控
   - 资源加载错误
   - js执行错误
   - promise错误

性能数据可以在页面加载完成后上报，尽量不要影响页面性能

错误数据在某个阶段统一上报

3. 用户信息收集
   - window.navigator
   - 浏览量或点击量
   - 页面停留时间

## 前端安全问题

1. XSS（跨站脚本攻击）：攻击者利用漏洞将恶意脚本注入到网站中，使得用户在浏览网站时受到攻击，可能导致数据泄露或盗取等问题。
2. CSRF（跨站请求伪造）：攻击者利用受害者在已登录的网站中的身份信息，发送伪造请求来进行一些恶意操作，例如盗取用户账户、篡改数据等等。
3. 点击劫持：攻击者通过将恶意网站覆盖在正常网站之上，诱骗用户进行点击，从而执行一些恶意操作。
4. 资源劫持：攻击者通过篡改资源文件（如CSS、JS文件），使得用户下载的资源文件中夹带了恶意代码。
5. SQL注入：攻击者利用漏洞在数据库中执行恶意SQL语句，以获取敏感数据或者篡改数据。

解决办法：

1. 防范XSS攻击：对用户输入的数据进行过滤和验证，避免直接将用户输入的内容展示在网页上，使用CSP（内容安全策略）等技术限制网页的资源获取。
2. 防范CSRF攻击：使用CSRF Token或者双重认证等技术来增强用户身份验证的安全性，避免攻击者冒用用户身份发起恶意请求。
3. 避免点击劫持：使用X-Frame-Options头部或者在前端页面中设置相应的防护措施，避免恶意网站覆盖在正常网站之上。
4. 避免资源劫持：使用HTTPS协议，避免资源文件被篡改，使用哈希等技术对资源文件进行验证。
5. 避免SQL注入：使用参数化查询或者ORM等技术来避免将用户输入的数据拼接在SQL语句中，以避免注入攻击。