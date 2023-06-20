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

- 几何计算
  - 向量操作
  - 矩阵操作
  - 四元数操作
  - 边界框计算
  - 几何转换和变换
  - 射线和平面计算
  - 几何插值和插值操作
- 时间管理
  - 时间戳的处理和转换
  - 时间间隔的计算
  - 动画和插值操作
  - 时间格式化和解析
- 事件系统
  - 事件的订阅和发布
  - 自定义事件的创建和处理
  - 事件监听和触发
- 坐标系转换
  - 经纬度和地理坐标系之间的转换
  - 笛卡尔坐标系和地理坐标系之间的转换
  - 屏幕坐标系和地理坐标系之间的转换
- 图形渲染
  - 颜色处理和转换
  - 图像纹理操作
  - 渲染状态的管理和设置
- 数据结构
  - 数组和列表操作
  - 堆和优先队列
  - 哈希表和字典操作
- 文件加载
  - 图片加载和处理
  - 文本文件加载和解析
  - 模型文件加载和渲染
- 地理空间计算
  - 距离计算和转换
  - 区域面积计算
  - 地形高度计算和查询
- 格式化和解析
  - 数值和字符串的格式化
  - 坐标和时间的解析
  - 数据格式的解析和转换
- 性能优化
  - 算法优化和数据结构的选择
  - 内存管理和资源释放
  - 渲染性能的优化技巧

这些只是Core模块的一部分

## Render

`Render` 模块涉及地理可视化场景的渲染和渲染管道的管理。该模块包含了与图形渲染相关的功能和类。以下是 `Render` 模块的一些主要部分和功能：

- **渲染管道（Render Pipeline）**: `Render` 模块定义了一个渲染管道，用于管理场景中各个渲染阶段的执行顺序和设置。渲染管道包括几何处理、光照计算、阴影投射、纹理渲染等多个阶段，以生成最终的可视化效果。
- **渲染状态（Render State）**: `Render` 模块提供了管理渲染状态的功能，包括深度测试、融合模式、剔除模式等。通过控制渲染状态，可以实现场景中的透明效果、深度排序等功能。
- **着色器程序（Shader Program）**: `Render` 模块定义了用于渲染的着色器程序。着色器程序包含了顶点着色器和片元着色器，用于处理场景中的几何形状和材质的渲染。
- **帧缓冲（Frame Buffer）**: `Render` 模块提供了帧缓冲对象的创建和管理，用于将渲染结果输出到特定的纹理或缓冲区中。
- **纹理渲染（Texture Rendering）**: `Render` 模块包含了将纹理应用于几何体并进行渲染的功能。这涉及将纹理映射到三维物体表面，并根据材质属性和光照计算生成最终的渲染结果。
- **光照和阴影（Lighting and Shadows）**: `Render` 模块定义了光照模型和阴影计算的功能。它包括了各种光源类型的支持（如平行光、点光源等），以及实现阴影投射的算法。
- **后期处理（Post-processing）**: `Render` 模块还提供了一些后期处理效果的功能，如景深效果、辉光效果、色彩校正等。这些效果可以在最终渲染之后应用于场景，以增强可视化效果。

## Scene

`Scene` 模块涉及地理可视化场景的管理、渲染和交互。该模块包含了与场景相关的功能和类。以下是 `Scene` 模块的一些主要部分和功能：

- **场景管理（Scene Management）**: `Scene` 模块提供了场景的创建和管理功能。它包含了场景的基本属性设置，如背景颜色、相机设置、光照设置等。开发者可以使用这些功能来创建自定义的可视化场景。
- **相机控制（Camera Control）**: `Scene` 模块允许对场景中的相机进行控制。它提供了相机的移动、旋转、缩放等操作，以及获取相机状态和属性的功能。
- **可视化对象（Visual Objects）**: `Scene` 模块提供了创建和管理可视化对象的功能。可视化对象可以是地形、模型、标记、几何体等，用于在场景中呈现地理数据和图形。
- **地形（Terrain）**: `Scene` 模块支持地形的加载和渲染。它提供了地形数据的管理、LOD（Level of Detail）控制、地形纹理贴图等功能。
- **图层管理（Layer Management）**: `Scene` 模块允许添加、移除和管理图层。图层可以是地图瓦片、影像、矢量数据等，用于叠加在场景中的可视化对象之上。
- **鼠标交互（Mouse Interaction）**: `Scene` 模块提供了与鼠标交互相关的功能。它允许捕获鼠标事件，如点击、拖拽、滚动等，并根据用户操作进行相应的场景交互。
- **屏幕空间操作（Screen Space Manipulation）**: `Scene` 模块支持屏幕空间的交互操作，如屏幕上的拾取、标记和绘制等。这些操作在屏幕空间中进行，而不依赖于场景的真实地理坐标。
- **场景渲染回调（Scene Rendering Callbacks）**: `Scene` 模块提供了场景渲染过程中的回调函数，允许开发者在渲染的不同阶段进行自定义的操作和效果。

## DataSources

`Data Source` 模块是用于加载和管理地理数据源的模块。它提供了一种统一的方式来加载、处理和显示不同类型的地理数据。以下是 `Data Source` 模块的一些主要部分和功能：

- **数据源管理（Data Source Management）**：`Data Source` 模块允许添加、移除和管理不同类型的地理数据源。数据源可以是矢量数据、栅格数据、点云数据等。
- **数据源加载（Data Source Loading）**：`Data Source` 模块支持异步加载各种类型的地理数据源。它提供了加载和解析数据的接口，使得开发者能够从不同的数据源（如文件、网络服务）中获取地理数据。
- **数据源可视化（Data Source Visualization）**：`Data Source` 模块允许对加载的地理数据进行可视化。它提供了处理和渲染地理数据的功能，使得数据可以在场景中以可视化的方式呈现。
- **数据源样式化（Data Source Styling）**：`Data Source` 模块允许对加载的地理数据进行样式化和符号化。开发者可以定义数据源的样式，如颜色、线宽、填充模式等，以使数据以符号化的方式显示。
- **数据源过滤（Data Source Filtering）**：`Data Source` 模块允许对加载的地理数据进行过滤和筛选。开发者可以定义过滤规则，以限制要显示或处理的数据的范围。
- **数据源事件（Data Source Events）**：`Data Source` 模块提供了一组事件，用于监听数据源的加载、更新和状态变化等。开发者可以通过监听这些事件来执行相应的操作或响应用户交互。
- **数据源属性查询（Data Source Property Query）**：`Data Source` 模块允许查询加载的地理数据源的属性信息。开发者可以获取数据源中的属性数据，如属性名称、类型、值等。
- **数据源更新（Data Source Updates）**：`Data Source` 模块支持对加载的地理数据源进行动态更新。开发者可以通过添加、删除或修改数据源中的数据来实现数据的实时更新和变化。

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