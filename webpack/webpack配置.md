## 代码分离

### 入口起点

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  mode:'development',//开发环境
  entry: { //入口文件
    index: {
      import:'./src/index.js',
      dependOn: 'shared'
    },
    another: {
      import:'./src/another-module.js',
      dependOn:'shared'
    },
    shared:'lodash'
  },
  devtool: 'inline-source-map',//开启source-map
  devServer:{
    static: './dist'
  },
  plugins:[
    new HtmlWebpackPlugin({ // 添加html文件并注入依赖
      title:'管理输出'
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname,'dist'),
    clean:true //每次构建清理dist目录旧文件
  },
  optimization:{//一个html设置多个入口时
    splitChunks:{
      chunks:'all'
    }
  }
}

```

### 动态导入

```js
async function getComponent(){
  const element = document.createElement("div")
  const {default:_} = await import('lodash')
  element.innerHTML = _.join(['Hello','webpack'],' ')
  return element
}
getComponent().then(component=>{
  document.body.appendChild(component)
})
```

