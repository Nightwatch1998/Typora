## 安装

### 下载安装包

- 安装nvm之前卸载以前安装的nodejs和nvm

- windows环境[下载](https://github.com/coreybutler/nvm-windows/releases)选择nvm-setup.exe，一路next，选择安装目录
- 安装完成后会自动在环境变量中添加`NVM_HOME`和`NVM_SYMLINK`

### 配置镜像源

```ruby
nvm node_mirror https://npm.taobao.org/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

## 原理

切换node版本时，会将当前node版本的可执行文件路径作为`NVM_SYMLINK`的引用中，这样，执行node 命令时，优先使用的是`NVM_SYMLINK`目录的可执行文件

## 使用

- `nvm install <version>` 安装指定nodejs版本
- `nvm uninstall <version>` 卸载指定版本
- `nvm use <version>` 切换到指定版本
- `nvm ls` 查看当前已安装的版本

