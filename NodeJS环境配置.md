## NodeJS  zip安装方式

### 下载并解压

[官网下载](https://nodejs.org/en/download/)解压至目标文件夹

### 添加环境变量

- PATH 中添加node安装路径

```
H:\Progarm Coding\nodejs\node-v18.15.0-win-x64
```

注：最新版中NODE_HOME和NODE_PATH已被弃用

### 配置全局安装路径和缓存

在nodejs安装目录下创建node_global而node_cached文件夹

```
npm config set prefix "H:\Progarm Coding\nodejs\node-v18.15.0-win-x64\node_global"
npm config set cache "H:\Progarm Coding\nodejs\node-v18.15.0-win-x64\node_cached"

```

检查是否设置成功

```
npm config list
```



### 设置镜像

使用淘宝镜像

```
npm config set registry https://registry.npm.taobao.org
```

### 验证

```
node -v
npm -v
npx -v
```

## nvm安装

### 下载安装包

在nvm的[GitHub页面](https://github.com/coreybutler/nvm-windows/releases)下载nvm安装包(nvm-setup.zip)

### 安装nvm

按照安装向导提示进行安装

### 验证

```
nvm -v
```

### nvm使用

- 列举可用的node版本

```
nvm list available
```

- 安装指定版本

```
nvm install 12
```

- 列举已安装版本

```
nvm list
```

- 使用指定版本

```
nvm use <node_version>
```

