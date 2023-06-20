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

## Renderer

## Scene

## DataSources

## Shader

## Documentation 文档

```
Documentation
├── Contributors 
│   ├── BuildGuide
│   ├── CLAs
│   ├── CodeReviewGuide
│   ├── CodingGuide
│   ├── CommittersGuide
│   ├── DocumentationGuide
│   ├── DracoModuleManagement
│   ├── MobileGuide
│   ├── PerformanceTestingGuide
│   ├── PresentersGuide
│   ├── ReleaseGuide
│   ├── TestingGuide
│   └── VSCodeGuide
├── CustomShaderGuide
├── FabricGuide
├── Images
├── OfflineGuide
└── Schemas
    └── Fabric
```



- Conrtibutors 各种手册
  - **CodingGuide** 编码指南
  - **DocumentationGuide** 文档指南
  - 性能测试指南
- CustomShaderGuide 自定义着色器

## 其他

### Specs 测试

- Data  测试数据
- TestWorkers 

### ThirdParty  第三方库

### Apps 应用示例

- Sandcastle

- TimelineDemo