# GeoServer 学习记录

## 基础

GeoServer 是一个基于Java的服务器，建立在GeoTools之上，用于查看和编辑GIS数据，内部集成了Openlayers。[官方文档](https://docs.geoserver.org/)

### 下载安装

[官网下载](https://sourceforge.net/projects/geoserver/files/)下载生产版本，解压至目标文件夹

- Platform Independent Binary 独立于平台的二进制文件
- Windows Installer windows 环境的jar包，必须有java环境 **开发环境**
- Web Archive  war包，可以丢在tomcat里启动  **生产环境**

windows环境安装 需要配置java环境

[linux环境下安装](https://docs.geoserver.org/latest/en/user/installation/linux.html)

### 启动

进入bin目录，执行命令：

```
startup.sh
```

浏览器中输入：

```
http://localhost:8080/geoserver
```

账户名密码：

```
用户名：admin，密码：geoserver
```

## 发布数据

### 发布图像

1. **数据准备**，放置在根目录data_dir/data下

2. **创建工作区**，指定名称和url

3. **创建一个数据存储**:数据源选择WorldImage,选择本地影像

4. **创建一个图层**:新建图层、选择数据、点击发布，进入配置页面

   - 基本参考信息：命名、标题、摘要
   - 坐标参考系统：强制声明会使用内部EPSG定义的WGS84，而非提供的 投影文件
   - 边框：由SRS计算，由native计算

   配置完成后，点击发布

   - 预览图层:

   注：SRS是指空间参考系统





