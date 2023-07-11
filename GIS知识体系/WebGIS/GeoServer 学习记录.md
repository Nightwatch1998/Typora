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

### 问题总结

#### 版本特性

- 版本2.22.x 需要Java8或者Java11环境
- 版本2.23.x以后 需要Java11或者Java17环境
- 版本2.6以前使用Jetty作为内置服务器，之后使用tomcat

#### geoserver跨域问题

修改geoserver\webapps\geoserver\WEB-INF\web.xml文件

将下面这两段取消注释

```xml
   <!-- Uncomment following filter to enable CORS in Jetty. Do not forget the second config block further down.
    <filter>
      <filter-name>cross-origin</filter-name>
      <filter-class>org.eclipse.jetty.servlets.CrossOriginFilter</filter-class>
      <init-param>
        <param-name>chainPreflight</param-name>
        <param-value>false</param-value>
      </init-param>
      <init-param>
        <param-name>allowedOrigins</param-name>
        <param-value>*</param-value>
      </init-param>
      <init-param>
        <param-name>allowedMethods</param-name>
        <param-value>GET,POST,PUT,DELETE,HEAD,OPTIONS</param-value>
      </init-param>
      <init-param>
        <param-name>allowedHeaders</param-name>
        <param-value>*</param-value>
      </init-param>
    </filter>
    -->

   <!-- Uncomment following filter to enable CORS
    <filter-mapping>
        <filter-name>cross-origin</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    -->
```



## 发布WMS服务

### 发布图像

1. **数据准备**，放置在数据目录目录E:\ProgramData\geoserver\data下

2. **创建工作区**，指定名称和url

3. **创建一个数据Store**:数据源选择WorldImage,选择本地影像

4. **创建一个图层**:新建图层、选择数据、点击发布，进入配置页面

   - 基本参考信息：命名、标题、摘要
   - 坐标参考系统：强制声明会使用内部EPSG定义的WGS84，而非提供的 投影文件
   - 边框：由SRS计算，由native计算

   配置完成后，点击发布

   - 预览图层:

   注：SRS是指空间参考系统

### 发布GeoPackage

和发布图像类似，store和数据源有所不同

### 发布图层组

1. 首先发布若干单个图层
2. 创建图层组，可以将同一工作空间的不同图层组合在一起，并调整顺序
3. 确定坐标系，生成边界

### 发布样式

1. 创建样式
2. 输入名称、选择工作空间、格式和图例
3. 选择样式内容，并生成配置文件xml，自定义样式文件
4. 在发布页面，找到对应的图层
5. 在图层预览中浏览

### 发布shapefile

1. 创建数据store，选择数据源shp文件，指定DBF 字符集
2. 在发布界面，由数据生成边界框，投影的和经纬度的

### 发布PostGIS table（补充）



[发布postgis表](https://docs.geoserver.org/latest/en/user/gettingstarted/postgis-quickstart/index.html)

## 发布WMTS服务

Geoserver内置了GeoWebCache服务，可以将图层提前切片保存

### 图层切片

- 在Tile Layers中找到对应的图层 点击seed/Truncate
- 编辑图层切片参数
- 点击submit创建切片任务
- 切出的地图瓦片在geoserver\gwc\huyadish_basemap目录
- 可在预览中查看服务URL

### Cesium中使用

```
// 原始URL
http://localhost:8080/geoserver/gwc/service/wmts/rest/huyadish:basemap/{style}/{TileMatrixSet}/{TileMatrix}/{TileRow}/{TileCol}?format=image/png

// cesium中修改
http://localhost:8080/geoserver/gwc/service/wmts/rest/huyadish:basemap/{style}/{TileMatrixSet}/{TileMatrixSet}:{TileMatrix}/{TileRow}/{TileCol}?format=image/png
```

使用案例

```js
// 本地发布的WMS服务,layers可选countries、SR_50M、basemap
const geoserverWMSProvider = new Cesium.WebMapServiceImageryProvider({
  url: 'http://localhost:8080/geoserver/huyadish/wms',
  layers: 'huyadish:countries',
  parameters: {
    service: 'WMS',
    // 这里要看geoserver发布服务支持的格式,png、gif、jpeg
    format: 'image/png', 
    // 这里1.1.1和1.1.0均可
    version: '1.1.1', 
    transparent: true
  }
})

// 本地发布的WMTS服务
const geoserverWMTSProvider = new Cesium.WebMapTileServiceImageryProvider({
  url: 'http://localhost:8080/geoserver/gwc/service/wmts/rest/huyadish:basemap/{style}/{TileMatrixSet}/{TileMatrixSet}:{TileMatrix}/{TileRow}/{TileCol}?format=image/png',
  layer: 'huyadish:basemap',
  style: '',
  format: 'image/png',
  tileMatrixSetID: 'EPSG:900913', //EPSG需要修改
  maximumLevel: 10,
})
```

