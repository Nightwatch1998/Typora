# Cesium 源码学习

cesium是一个开源的地理可视化库，用于创建基于地球的应用程序。

版本1.106.1

## 项目结构

- 根目录结构

```
cesium
├── Apps //这个文件夹包含了一些 Cesium 的示例应用程序，可以用于学习和参考
├── Documentation //Cesium 的文档目录，包含了详细的 API 文档、教程和示例代码
├── Specs // 存放了 Cesium 的单元测试规格文件
├── ThirdParty //引用的第三方库
├── Tools // 封装了一些工具
├── launches // 配置ROS节点
├── packages //Cesium 源码的目录，包含核心功能和模块
└── travis // 一些自动化脚本
```

- packages 目录结构

```
packages
├── engine
│   ├── Source
│   │   ├── Assets // 包含了一些默认的资源文件，如纹理、模型等
│   │   ├── Core // 核心模块，提供了基本的几何、矩阵运算、时间管理等功能
│   │   ├── DataSources // 数据源相关
│   │   ├── Renderer // 渲染引擎，负责将地球表面、模型、图像渲染到屏幕
│   │   ├── Scene // 场景管理的模块，如相机控制、地形渲染、图像处理等
│   │   ├── Shaders // 存放了用于渲染的 GLSL 着色器代码
│   │   ├── ThirdParty // 使用的第三方库
│   │   ├── Widget
│   │   ├── Workers
│   │   └── WorkersES6
│   └── Specs
│       ├── Core
│       ├── DataSources
│       ├── Renderer
│       ├── Scene
│       └── Widget
└── widgets
    └── Source // 控件源码
```

## Core

Core模块功能

### 数学计算工具

  - 向量操作
  - 矩阵操作
  - 四元数操作
  - 边界框计算
  - 几何转换和变换
  - 射线和平面计算
  - 几何插值和插值操作
  - 样条曲线
  - 多项式逼近

### 地图场景相关

- Geometry基类和各种Geometry的定义
- 地形的创建、填充、编码、状态等等
- 视椎体几何体
- 地形瓦片

### 地理编码与计算

- 地球定向参数
- 椭圆和椭球体，椭球大地线、椭球等角线、椭球切平面等等
- 方位角
- 采样器
- 地图投影

### 数据结构与算法

- 深浅拷贝
- 对象的合并
- 数组和列表操作
- 堆和优先队列
- 双端优先队列
- 双向链表
- 哈希表和字典操作
- 二分搜索

### 时间管理

  - 时间戳的处理和转换
  - 时间间隔的计算
  - 动画和插值操作
  - 时间格式化和解析

### 事件系统

  - 事件的订阅和发布
  - 自定义事件的创建和处理
  - 事件监听和触发
  - 屏幕空间事件
### 坐标系

  - 笛卡尔2、3、4维坐标系
  - 经纬度和地理坐标系之间的转换
  - 笛卡尔坐标系和地理坐标系之间的转换
  - 屏幕坐标系和地理坐标系之间的转换

### 图形渲染

  - 颜色处理和转换
  - 图像纹理操作
### 文件加载

  - 图片加载和处理
  - 文本文件加载和解析
  - 模型文件加载和渲染
### 格式化和解析

  - 数值和字符串的格式化
  - 坐标和时间的解析
  - 数据格式的解析和转换

主要包括四大块：数学工具和函数、地球椭球模型、地图场景元素、数据结构和算法。

## Render（进阶补充）

`Render` 模块涉及地理可视化场景的渲染和渲染管道的管理。该模块包含了与图形渲染相关的功能和类。以下是 `Render` 模块的一些主要部分和功能：

- **渲染管道（Render Pipeline）**: `Render` 模块定义了一个渲染管道，用于管理场景中各个渲染阶段的执行顺序和设置。渲染管道包括几何处理、光照计算、阴影投射、纹理渲染等多个阶段，以生成最终的可视化效果。
- **渲染状态（Render State）**: `Render` 模块提供了管理渲染状态的功能，包括深度测试、融合模式、剔除模式等。通过控制渲染状态，可以实现场景中的透明效果、深度排序等功能。
- **着色器程序（Shader Program）**: `Render` 模块定义了用于渲染的着色器程序。着色器程序包含了顶点着色器和片元着色器，用于处理场景中的几何形状和材质的渲染。
- **帧缓冲（Frame Buffer）**: `Render` 模块提供了帧缓冲对象的创建和管理，用于将渲染结果输出到特定的纹理或缓冲区中。
- **纹理渲染（Texture Rendering）**: `Render` 模块包含了将纹理应用于几何体并进行渲染的功能。这涉及将纹理映射到三维物体表面，并根据材质属性和光照计算生成最终的渲染结果。
- **光照和阴影（Lighting and Shadows）**: `Render` 模块定义了光照模型和阴影计算的功能。它包括了各种光源类型的支持（如平行光、点光源等），以及实现阴影投射的算法。
- **后期处理（Post-processing）**: `Render` 模块还提供了一些后期处理效果的功能，如景深效果、辉光效果、色彩校正等。这些效果可以在最终渲染之后应用于场景，以增强可视化效果。
- 缓冲区、帧缓冲区
- 清除命令、计算命令、绘制命令
- 上下文
- 立方体贴图
- 采样器
- 着色器的构建器，着色器缓存。便于编写着色器程序
- 纹理缓存
- 顶点数组

## Scene（主要API）

`Scene` 模块涉及地理可视化场景的管理、渲染和交互。该模块包含了与场景相关的功能和类。以下是 `Scene` 模块的一些主要部分和功能：

### GLTF相关（补充）

- gltf管线
- 各种gltf加载器
- 添加缓冲区、默认值、额外信息
- 添加、移除扩展

### Model相关（补充）

- 模型动画
- 模型实例
- 模型外观
- 模型网格
- 模型节点

### Camera相机

Camera源码4000行，定义了大量使用相机的函数

Camera相机、相机事件、飞行路径

```js
// Camera构造函数
function Camera(scene) {
  if (!defined(scene)) {
    throw new DeveloperError("scene is required.");
  }
  this._scene = scene;

  // 变换矩阵
  this._transform = Matrix4.clone(Matrix4.IDENTITY);
  this._invTransform = Matrix4.clone(Matrix4.IDENTITY);
  this._actualTransform = Matrix4.clone(Matrix4.IDENTITY);
  this._actualInvTransform = Matrix4.clone(Matrix4.IDENTITY);
  this._transformChanged = false;

  // 相机的属性
  this.position = new Cartesian3(); // 相机位置
  this.direction = new Cartesian3(); // 视图方向
  this.up = new Cartesian3(); 
  this.right = new Cartesian3();
  this.frustum = new PerspectiveFrustum(); // 视图区域

  // 一些默认姿态参数
  this.defaultMoveAmount = 100000.0;
  this.defaultLookAmount = Math.PI / 60.0;
  this.defaultRotateAmount = Math.PI / 3600.0;
  this.defaultZoomAmount = 100000.0;
  this.maximumZoomFactor = 1.5;

  // 运动相关的事件
  this._moveStart = new Event();
  this._moveEnd = new Event();
  this._changed = new Event();

  // 视图矩阵
  this._viewMatrix = new Matrix4();
  this._invViewMatrix = new Matrix4();
  updateViewMatrix(this);

  // 场景模式
  this._mode = SceneMode.SCENE3D;
  this._modeChanged = true;
  // 投影
  const projection = scene.mapProjection;
  this._projection = projection;
  this._maxCoord = projection.project(
    new Cartographic(Math.PI, CesiumMath.PI_OVER_TWO)
  );
  this._max2Dfrustum = undefined;

  // 设置默认视图
  rectangleCameraPosition3D(
    this,
    Camera.DEFAULT_VIEW_RECTANGLE,
    this.position,
    true
  );

  let mag = Cartesian3.magnitude(this.position);
  mag += mag * Camera.DEFAULT_VIEW_FACTOR;
  Cartesian3.normalize(this.position, this.position);
  Cartesian3.multiplyByScalar(this.position, mag, this.position);
}

// 相机航线预加载
Camera.prototype.canPreloadFlight = function () {}
// 更新相机的变化
Camera.prototype._updateCameraChanged = function () {}

// 定义相机原型对象
Object.defineProperties(Camera.prototype, {
  // 变换矩阵
  transform: {
    get: function () {
      return this._transform;
    },
  },
  // 逆变换矩阵
  inverseTransform: {
    get: function () {
      updateMembers(this);
      return this._invTransform;
    },
  },
  // 视图矩阵
  viewMatrix: {
    get: function () {
      updateMembers(this);
      return this._viewMatrix;
    },
  },
  // 相机制图位置
  positionCartographic: {
    get: function () {
      updateMembers(this);
      return this._positionCartographic;
    },
  },
  // 相机的世界坐标
  positionWC: {
    get: function () {
      updateMembers(this);
      return this._positionWC;
    },
  },
  // 相机的方向参数
  directionWC: {
    get: function () {
      updateMembers(this);
      return this._directionWC;
    },
  },
  // 相机的姿态
  heading: {
    get: function () {
      return undefined;
    },
  },
  pitch: {
    get: function () {
      return undefined;
    },
  },
  roll: {
    get: function () {
      return undefined;
    },
  },

  // 相机移动相关的事件
  moveStart: {
    get: function () {
      return this._moveStart;
    },
  },
  moveEnd: {
    get: function () {
      return this._moveEnd;
    },
  },
  changed: {
    get: function () {
      return this._changed;
    },
  },
});

// 设置相机的位置、朝向和变换矩阵
Camera.prototype.setView = function (options) {}
// 相机飞回Home视图
Camera.prototype.flyHome = function (duration) {}
// 世界坐标转相机坐标
Camera.prototype.worldToCameraCoordinates = function (cartesian, result) {}

// 沿着某个方向运移动
Camera.prototype.move = function (direction, amount) {} 
// 相机看向目标位置
Camera.prototype.look = function (axis, angle) {} 
// 从目标位置观察
Camera.prototype.lookAt = function (target, offset) {}
// 相机沿着坐标轴旋转
Camera.prototype.rotate = function (axis, angle) {} 

// 拉近视角
Camera.prototype.zoomIn = function (amount) {}
// 拉远视角
Camera.prototype.zoomOut = function (amount) {}

// 还有很多其他函数补充
```

### Primitive基本元素

primitive描述场景中的几何图形，可以是单一的也可以是多个。primitive将geometry和appearance结合，appearance包含材质和渲染状态。

将多个实例组合为一个primitive可以提升性能

2500行

### Scene对象

4000多行

### Globe对象

### Material对象

### 各种地图Provider

### Scene场景内容

- Light光源
- 粒子系统
- 阴影
- 地形、地形数据、地形编码、地形填充网格
- 纹理、纹理缓存
- 地球、月球、太阳
- 场景模式、场景变换

### 几何图形定义

- 平面、裁剪平面
- 点、点云
- 多边形、多边形几何体
- 折线、折线几何体
- 球体、球面

### Cesium 3D tile

- 3D tile 上下文
- 3D tile 集合的操作
- 3D tile 样式

- Tileset3DTile 瓦片集合
  - 上下文
  - 要素
  - 层次结构
  - 标签
  - 图元

### 其他

- 图层
  - 各种地图Provider
- 隐式内容
  - 隐式3D  tile
  - 隐式子树
- 各种数据加载器loader
- 后期处理

投影、坐标系等等

Scene模块主要包含了模型、gltf、3Dtiles、几何图形、场景等等。

## DataSources

`Data Source` 模块是用于加载和管理地理数据源的模块。它提供了一种统一的方式来加载、处理和显示不同类型的地理数据。以下是 `Data Source` 模块的一些主要部分和功能：

### 定义几何图形与更新器

几何图形包含：

- 广告牌
- 3Dtileset
- 圆柱、椭圆、椭球
- 平面、多边形、折线

以椭球为例：

- 首先是定义几何图形

```js
// 定义了椭球几何体的属性，并进行属性合并
function EllipsoidGraphics(options) {
  this._definitionChanged = new Event();
  this._show = undefined;
  this._showSubscription = undefined;
  this._radii = undefined;
  ...
  this.merge(defaultValue(options, defaultValue.EMPTY_OBJECT));
}

// 在原型对象上定义属性描述符
Object.defineProperties(EllipsoidGraphics.prototype, {
  definitionChanged: {
    get: function () {
      return this._definitionChanged;
    },
  },
  // 封装了属性描述符函数
  show: createPropertyDescriptor("show"),
  radii: createPropertyDescriptor("radii")
  ...
})

// 定义clone函数
EllipsoidGraphics.prototype.clone = function (result) {
  if (!defined(result)) {
    return new EllipsoidGraphics(this);
  }
  result.show = this.show;
  result.radii = this.radii;
  result.innerRadii = this.innerRadii;
  ...
  return result;
};

// 属性合并函数
EllipsoidGraphics.prototype.merge = function (source) {
  //>>includeStart('debug', pragmas.debug);
  if (!defined(source)) {
    throw new DeveloperError("source is required.");
  }
  //>>includeEnd('debug');

  this.show = defaultValue(this.show, source.show);
  this.radii = defaultValue(this.radii, source.radii);
  this.heightReference = defaultValue(
    this.heightReference,
    source.heightReference
  );
  this.distanceDisplayCondition = defaultValue(
    this.distanceDisplayCondition,
    source.distanceDisplayCondition
  );
  ...
};
// 导出几何体对象
export default EllipsoidGraphics;  
```

- 然后是椭球的几何图形更新器

```js
// 封账椭球体几何属性
function EllipsoidGeometryOptions(entity) {
  this.id = entity;
  this.vertexFormat = undefined;
  this.radii = undefined;
  ...
}

// 定义更新器
function EllipsoidGeometryUpdater(entity, scene) {
  // 绑定基类更新器的this，并传递参数
  GeometryUpdater.call(this, {
    entity: entity,
    scene: scene,
    geometryOptions: new EllipsoidGeometryOptions(entity),
    geometryPropertyName: "ellipsoid",
    observedPropertyNames: [
      "availability",
      "position",
      "orientation",
      "ellipsoid",
    ],
  });

  this._onEntityPropertyChanged(
    entity,
    "ellipsoid",
    entity.ellipsoid,
    undefined
  );
}
    
// 定义原型属性
Object.defineProperties(EllipsoidGeometryUpdater.prototype, {
  terrainOffsetProperty: {
    get: function () {
      return this._terrainOffsetProperty;
    },
  },
});
// 创建几何体填充实例
EllipsoidGeometryUpdater.prototype.createFillGeometryInstance = function(){}
// 创建几何体外边线实例
EllipsoidGeometryUpdater.prototype.createOutlineGeometryInstance = function(){}
// 计算中心
EllipsoidGeometryUpdater.prototype._computeCenter
// 是否隐藏
EllipsoidGeometryUpdater.prototype._isHidden
// 是否开启动态
EllipsoidGeometryUpdater.prototype._isDynamic
// 设置静态属性
EllipsoidGeometryUpdater.prototype._setStaticOptions
// 属性更新事件
EllipsoidGeometryUpdater.prototype._onEntityPropertyChanged = heightReferenceOnEntityPropertyChanged;
// 动态几何更新器
EllipsoidGeometryUpdater.DynamicGeometryUpdater = DynamicEllipsoidGeometryUpdater;
// 定义更新函数
DynamicEllipsoidGeometryUpdater.prototype.update = function(){}
```

### 可视化器

包含广告牌、标签、模型、路径、点、几何体等等

以几何体为例

```js
// 引入各种updater
const geometryUpdaters = [
  BoxGeometryUpdater,
  CylinderGeometryUpdater,
  CorridorGeometryUpdater,
  EllipseGeometryUpdater,
  EllipsoidGeometryUpdater,
  PlaneGeometryUpdater,
  PolygonGeometryUpdater,
  PolylineVolumeGeometryUpdater,
  RectangleGeometryUpdater,
  WallGeometryUpdater,
];

// 构建几何体updater的几何对象
function GeometryUpdaterSet(entity, scene) {
  this.entity = entity;
  this.scene = scene;
  const updaters = new Array(geometryUpdaters.length);
  // 几何体发生变化的事件
  const geometryChanged = new Event();
  // 触发事件，并传递几何体参数
  function raiseEvent(geometry) {
    geometryChanged.raiseEvent(geometry);
  }
  const eventHelper = new EventHelper();
  // 循环场景更新器实例
  for (let i = 0; i < updaters.length; i++) {
    const updater = new geometryUpdaters[i](entity, scene);
    eventHelper.add(updater.geometryChanged, raiseEvent);
    updaters[i] = updater;
  }
  this.updaters = updaters;
  this.geometryChanged = geometryChanged;
  this.eventHelper = eventHelper;
  //  注册实体定义变化的订阅
  this._removeEntitySubscription = entity.definitionChanged.addEventListener(
    GeometryUpdaterSet.prototype._onEntityPropertyChanged,
    this
  );
}

// 属性改变触发
GeometryUpdaterSet.prototype._onEntityPropertyChanged = function(){}
// 封装foreach函数
GeometryUpdaterSet.prototype.forEach = function(){}
// 销毁函数
GeometryUpdaterSet.prototype.destory = function(){}

// 定义visualizer构造器
function GeometryVisualizer(
  scene,
  entityCollection,
  primitives,
  groundPrimitives
) {}

// 更新函数，更新所有的pimitives
GeometryVisualizer.prototype.update = function(){}
// 获取边界球
GeometryVisualizer.prototype.getBoundingSphere = function(){}
// 销毁所有的primitives
GeometryVisualizer.prototype.destory = function(){}

// 定义了若干事件
GeometryVisualizer._onGeometryChanged = function(){}
GeometryVisualizer._onCollectionChanged = function(){}

// 最后导出
export default GeometryVisualizer;
```

### Entity

Entity是cesium中的重要概念，它可以包含多个几何体，用于描述场景中的对象或者元素。

primitives用于绘制基本的几何形状，例如点、线、面

```js
// Entity构造函数
function Entity(options) {
  options = defaultValue(options, defaultValue.EMPTY_OBJECT);

  let id = options.id;
  if (!defined(id)) {
    id = createGuid();
  }

  this._availability = undefined;
  this._id = id;
  this._definitionChanged = new Event();
  this._name = options.name;
  this._show = defaultValue(options.show, true);
  this._parent = undefined;
  this._propertyNames = [
    "billboard",
    "box",
    //...
  ];

  this._billboard = undefined;
  this._billboardSubscription = undefined;
  this._box = undefined;
  this._boxSubscription = undefined;
  //...
  this._children = [];

  this.entityCollection = undefined;

  this.parent = options.parent;
  this.merge(options);
}

// 递归更新元素的显示
function updateShow(entity, children, isShowing) {
  const length = children.length;
  for (let i = 0; i < length; i++) {
    const child = children[i];
    const childShow = child._show;
    const oldValue = !isShowing && childShow;
    const newValue = isShowing && childShow;
    if (oldValue !== newValue) {
      updateShow(child, child._children, isShowing);
    }
  }
  entity._definitionChanged.raiseEvent(
    entity,
    "isShowing",
    isShowing,
    !isShowing
  );
}

// 定义若干属性
Object.defineProperties(Entity.prototype, {
  availability: createRawPropertyDescriptor("availability"),
  id: {
    get: function () {
      return this._id;
    },
  },
  definitionChanged: {
    get: function () {
      return this._definitionChanged;
    },
  },
  name: createRawPropertyDescriptor("name"),
  show: {
    get: function () {
      return this._show;
    },
    set: function (value) {
      //>>includeStart('debug', pragmas.debug);
      if (!defined(value)) {
        throw new DeveloperError("value is required.");
      }
      //>>includeEnd('debug');

      if (value === this._show) {
        return;
      }

      const wasShowing = this.isShowing;
      this._show = value;
      const isShowing = this.isShowing;

      if (wasShowing !== isShowing) {
        updateShow(this, this._children, isShowing);
      }

      this._definitionChanged.raiseEvent(this, "show", value, !value);
    },
  },
  // 各种几何图形
  billboard: createPropertyTypeDescriptor("billboard", BillboardGraphics),
  box: createPropertyTypeDescriptor("box", BoxGraphics),
  corridor: createPropertyTypeDescriptor("corridor", CorridorGraphics),
  //...
})

// 给定时间，判断是否可访问
Entity.prototype.isAvailable = function (time) {}
// 添加属性
Entity.prototype.addProperty = function (propertyName) {}
// 移除属性
Entity.prototype.removeProperty = function (propertyName) {}
// 融合默认属性
Entity.prototype.merge = function (source) {}
// 计算模型矩阵
Entity.prototype.computeModelMatrix = function (time, result) {}


Entity.supportsMaterialsforEntitiesOnTerrain = function (scene) {}
Entity.supportsPolylinesOnTerrain = function (scene) {}
```



### 矢量数据源（补充）

- KML数据加载与展示

- GeoJson数据源
- GPX数据源
- 自定义数据源

### 其他

- 属性：位置、材质、速度等等

## Shader

- **着色器程序管理（Shader Program Management）**：`Shader` 模块提供了着色器程序的创建、编译和链接功能。它允许开发者定义和管理顶点着色器和片元着色器，并将它们组合成可用的着色器程序。
- **着色器程序输入（Shader Program Inputs）**：`Shader` 模块定义了着色器程序的输入变量，如顶点坐标、法线、纹理坐标等。开发者可以通过定义这些输入变量来与渲染管道中的数据进行交互。
- **着色器程序输出（Shader Program Outputs）**：`Shader` 模块定义了着色器程序的输出，如片元颜色、深度值等。开发者可以通过定义这些输出来指定着色器程序的渲染结果。
- **着色器程序内置变量（Shader Program Built-in Variables）**：`Shader` 模块提供了一些内置变量，用于访问渲染管道中的一些常用信息。例如，内置变量可以访问相机信息、光照信息、材质属性等。
- **着色器程序宏定义（Shader Program Macros）**：`Shader` 模块支持宏定义，允许开发者在着色器程序中定义和使用宏。宏定义可以用于条件编译、预处理指令等，以实现更灵活的着色器编程。
- **着色器程序内置函数（Shader Program Built-in Functions）**：`Shader` 模块提供了一些内置函数，用于在着色器程序中执行常见的计算和操作。这些内置函数可以用于矩阵操作、向量运算、纹理采样等。
- **着色器程序扩展（Shader Program Extensions）**：`Shader` 模块支持着色器程序的扩展功能。开发者可以使用扩展来添加额外的着色器特性和效果，以满足特定的渲染需求。
- **着色器程序预编译（Shader Program Precompilation）**：`Shader` 模块支持着色器程序的预编译功能。这可以提前编译着色器程序，以加快渲染过程中的加载和初始化速度。

## Documentation 文档

```
Documentation
├── Contributors 
│   ├── BuildGuide // 在本地构建和运行文档
│   ├── CodingGuide // JS和GLSL编码规范，设计、可维护性和性能的最佳实践
│   ├── DocumentationGuide // 如何写出好的参考文档
│   ├── PerformanceTestingGuide // 测量运行时性能的最佳实践
│   ├── TestingGuide // 如何编写和运行cesium测试
│   └── VSCodeGuide // vscode 相关设置
├── CustomShaderGuide // 自定义着色器指南
├── FabricGuide // 材质指南
├── OfflineGuide // 离线使用指南
└── Schemas
    └── Fabric
```



## 其他

### Specs 测试

- Data  测试数据
- TestWorkers 

### ThirdParty  第三方库

### Apps 应用示例

- Sandcastle

- TimelineDemo