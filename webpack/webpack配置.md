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

### 预获取prefetch

```js
import(/* webpackPrefetch: true */ './path/to/LoginModal.js');
```

### 预加载preload

```js
import(/* webpackPreload: true */ 'ChartingLibrary');
```

## 缓存

- 输出文件的文件名，用hash表示文件名

```js
filename: '[name].[contenthash].js',
```

- 将runtime代码拆分为一个单独的chunk

```js
  optimization:{
    runtimeChunk:'single'
  }
```

- 模块标识符,vendor前后构建hash不变

```js
moduleIds: 'deterministic',
```

## 创建一个Library（需要了看）

## 构建性能(内容较多，专门看)

## 模块热替换

- 常用配置(推荐)

```js
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: {
       app: './src/index.js',
      print: './src/print.js',
    },
    devtool: 'inline-source-map',
    devServer: {
      static: './dist',
     hot: true,
    },
    plugins: [
      new HtmlWebpackPlugin({
        title: 'Hot Module Replacement',
      }),
    ],
    output: {
      filename: '[name].bundle.js',
      path: path.resolve(__dirname, 'dist'),
      clean: true,
    },
  };
```

- 指定入口的方式

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const webpack = require("webpack")

module.exports = {
  // mode:'development',//开发环境
  entry: { //入口文件
    app: './src/index.js',
    hot: 'webpack/hot/dev-server.js',
    client: 'webpack-dev-server/client/index.js?hot=true&live-reload=true'
  },
  devtool:'inline-source-map',
  devServer:{
    static:'./dist',
    hot:false,
    client:false
  },
  plugins:[
    new HtmlWebpackPlugin({
      title:"Hot Module Replacement"
    }),
    new webpack.HotModuleReplacementPlugin()
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname,'dist'),
    clean:true //每次构建清理dist目录旧文件
  }
}
```

- css热更新

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode:'development',
  entry: {
    app: './src/index.js',
    print: './src/print.js',
  },
  devtool: 'inline-source-map',
  devServer: {
    static: false,
    hot: true,
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:['style-loader','css-loader']
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Hot Module Replacement',
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
};
```

## Tree shaking

- package.json中配置sideEffects

```json
{
  "name": "your-project",
  "sideEffects": ["./src/some-side-effectful-file.js", "*.css"]
}
```

- 将函数调用标记为无副作用

```js
/*#__PURE__*/ double(55);
```

## 环境配置

### webpack.common.js

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    app: './src/index.js',
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Production',
    })
  ],
  output: {
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  }
};
```

### webpack.dev.js

```js
const {merge} = require('webpack-merge')
const common = require("./webpack.common.js")

module.exports = merge(common,{
  mode:'development',
  devtool:'inline-source-map',
  devServer:{
    static:'./dist'
  }
})
```

### webpack.prod.js

```js
const {merge} = require('webpack-merge')
const common = require("./webpack.common.js")

module.exports = merge(common,{
  mode:'production',
  devtool:'source-map'
})
```

## Shimming 预置依赖

- 预置全局依赖，如果在多个文件中使用了，将会比较有用

```js
const path = require('path');
const webpack = require('webpack')

module.exports = {
  entry: {
    app: './src/index.js',
  },
  plugins: [
    new webpack.ProvidePlugin({
      _:'lodash'
    })
  ],
  output: {
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  }
};
```

- 配置数组路径，按需引入

```js
const path = require('path');
const webpack = require('webpack')

module.exports = {
  mode:'development',
  entry: {
    app: './src/index.js',
  },
  plugins: [
    new webpack.ProvidePlugin({
      join:['lodash','join']
    })
  ],
  output: {
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  }
};
```

## TypeScript

### tsconfig.json

```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "sourceMap": true,
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx":"react",
    "allowJs": true,
    "moduleResolution": "node"
  }
}
```

### webpack.config.js

```js
const path = require('path');
const webpack = require('webpack')

module.exports = {
  mode:'development',
  entry: {
    app: './src/index.ts',
  },
  devtool:"inline-source-map",
  module:{
    rules:[
      {
        test:/\.tsx?$/,
        use:'ts-loader',
        exclude:/node_modules/
      }
    ]
  },
  resolve:{
    extensions:['.tsx','.ts','.js']
  },
  output: {
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  }
};
```

