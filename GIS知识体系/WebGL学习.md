## 模型、视图、投影矩阵

各种矩阵用途：

- 模型矩阵：获取原始模型数据，并在3D世界空间移动。
- 投影矩阵：将世界空间坐标转换为裁剪空间坐标。模拟相机的效果。
- 视图矩阵：负责移动场景中的对象以模拟相机位置的变化。改变观察者当前能够看到的内容。

### 裁剪空间

这是一个中心点位于 (0, 0, 0)，角落范围在 (-1, -1, -1) 到 (1, 1, 1) 之间，2 个单位宽的立方体。该剪裁空间被压缩到一个二维空间并栅格化为图像（可以理解为投影）。

顶点着色器将点转换到裁剪空间的特殊坐标系上。

裁剪空间内的坐标为归一化坐标。只有裁剪空间内的元素才会被渲染。

裁剪空间中z+远离观察者

<img src="https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/WebGL_model_view_projection/clip_space_graph.svg" alt="A 3d graph showing clip space in WebGL." style="zoom: 25%;" />

### 齐次坐标

(x,y,z)增加第四个坐标(x,y,z,w)变为齐次坐标，w作为其他分量的除数。将透视的概念引入坐标系，允许两条平行线在延伸到远方时相交。

### 模型转换

将模型数据和其他坐标转换到裁剪空间。

例如立方体表面颜色和构成单个多边形的顶点位置顺序组成。这些位置和颜色存储在GL缓冲区，分发到着色器分别进行操作。

计算单个模型矩阵。该矩阵表示要在组成模型的每个点上执行转换。

### 除以W

获取立方体模型透视的简单方法，将Z坐标复制到W上，w可以描述透视。

### 简单投影

```
// 单位阵
var identity = [
  1, 0, 0, 0,
  0, 1, 0, 0,
  0, 0, 1, 0,
  0, 0, 0, 1,
];

MDN.multiplyPoint(identity, [2, 3, 4, 1]);
//> [2, 3, 4, 1]

// 移动一格
var copyZ = [
  1, 0, 0, 0,
  0, 1, 0, 0,
  0, 0, 1, 1,
  0, 0, 0, 0,
];

MDN.multiplyPoint(copyZ, [2, 3, 4, 1]);
//> [2, 3, 4, 4]

// 缩放因子
var scaleFactor = 0.5;

var simpleProjection = [
  1, 0, 0, 0,
  0, 1, 0, 0,
  0, 0, 1, scaleFactor,
  0, 0, 0, scaleFactor,
];

MDN.multiplyPoint(simpleProjection, [2, 3, 4, 1]);
//> [2, 3, 4, 2.5]
```

通过scaleFactor，可以控制投影的远近程度：

```GLSL
// 确保以相反的顺序读取转换矩阵
gl_Position = projection * model * vec4(position, 1.0);
```

### 透视矩阵（推导下）

[OpenGL透视投影](https://www.songho.ca/opengl/gl_projectionmatrix.html)

透视投影将视椎体映射为立方体。

已知信息：

- 假设相机位置位于原点，t、b、l、r分别位于视椎体近面上下左右四条边的y、y、x、x坐标,n为近面z坐标，f为远面坐标。

- 标准化设备（NDC）坐标：(-1,-1,-1)到(1,1,1)

是由数学公式推导出来的：

- 首先，将视椎体的视野坐标Xe,Ye投影到近裁剪面Xp,Yp
- 由于视野坐标是右手坐标系，NDC为左手坐标系，所以Z要反向，Wc可定义为-Ze，**矩阵第四行确定**。
- 将投影坐标Xp,Yp线性变换为NDC近平面坐标Xn,Yn
- 然后得到Xn与Xe,Ze，Yn与Ye,Ze的推算关系，**确定矩阵的前两行**
- 计算矩阵第四行

```GLSL
MDN.perspectiveMatrix = function(fieldOfViewInRadians, aspectRatio, near, far) {

  var f = 1.0 / Math.tan(fieldOfViewInRadians / 2);
  var rangeInv = 1 / (near - far);

  return [
    f / aspectRatio, 0,                          0,   0,
    0,               f,                          0,   0,
    0,               0,    (near + far) * rangeInv,  -1,
    0,               0,  near * far * rangeInv * 2,   0
  ];
}

```

### 视图矩阵

## 教程

