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

### Plugins实现思路？

webpack基于发布订阅模式，在运行的生命周期会广播出许多事件，插件监听这些事件，就可以在特定的阶段执行自己的插件任务。

生命周期钩子:comiler-hooks

依赖钩子:compilation hooks

### 文件监听的原理

带上--watch参数

轮训判断文件的最后编辑时间时候发生变化。如果某个文件发生了变化，并不会立即告诉监听者，先缓存起来，

等aggregateTimeout后在执行

### 热更新原理

热更新HMR，又称为热替换，不用刷新浏览器而将新变更的模块替换旧的模块

WDS与浏览器之间维护了一个Websocket，本地资源变化时，WDS会向浏览器推送更新，并带上构建时的hash，

客户端进行diff，找到差异后，向WDS发起Ajax请求，来获取更改内容。

### 对bundle体积进行监控和分析

### 文件指纹是什么？

打包后输出的文件名的后缀

- hash：和整个项目的构建有关
- ChunkHash：不同的entry，不同的chunkhash
- Contenthash：根据文件内容定义

## 模块化

Commonjs

ES6

AMD

## 性能优化