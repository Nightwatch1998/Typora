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

## 其他构建工具

### rollup

优点：

- 代码效率简洁、效率更高
- 默认支持Tree-shaking

缺点：

- 加载其他类型资源，导入CommonJS模块，都需要插件完成
- 不支持HMR
- 需要使用第三方模块

### parcel

自动安装依赖，支持热替换

### vite

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

nodejs采用CommonJS规范。每个文件一个模块，有自己的作用域。文件里定义的变量，函数，类都是私有的。对其他文件不可见。

基本语法：

- 暴露模块：`module.exports = value`或`exports.xxx = value`
- 引入模块：`require(xxx)`,如果是第三方模块，xxx为模块名；如果是自定义模块，xxx为模块文件路径

模块加载机制：引入的是模块里值的拷贝，**同步加载**

### ES6

ES6模块的设计思想是尽量静态化，使得编译时就能确定模块的依赖关系

基本语法：

```js
/** 定义模块 math.js **/
var basicNum = 0;
var add = function (a, b) {
    return a + b;
};
export { basicNum, add };
/** 引用模块 **/
import { basicNum, add } from './math';
function test(ele) {
    ele.textContent = add(99 + basicNum);
}
```

ES6模块与CommonJS模块的差异：

- CommonJS模块输出的是值的拷贝，ES6输出的是引用
- CommonJS模块是运行时加载，ES6是编译时输出接口
- CommonJS动态语法写在判断里，ES6静态语法只能写在顶层

### AMD

浏览器端一般采用AMD规范

基本语法：

```js
// 定义没有依赖的模块
define(function(){
   return 模块
})

// 定义有依赖的模块
define(['module1', 'module2'], function(m1, m2){
   return 模块
})

// 
require(['module1', 'module2'], function(m1, m2){
   使用m1/m2
})
```



## 性能优化