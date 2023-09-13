# Cesium中模型的空间变换

本文使用Cesium版本为1.92.0

加载3dtiles模型时，最好在处理数据时就设置好模型的定位点和投影坐标系信息。如果在Cesium中还有偏差，可通过空间变换进行修正。

Cesium中模型的变换矩阵是4阶矩阵，因此定义的旋转、平移、缩放也要是4阶矩阵，Cesium也提供了使用3阶矩阵的优化算法(目前没走通)

首先加载模型：

```js
var tileset = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({
	url: 'Scene/Pipeline/tileset.json',
	maximumScreenSpaceError: 2,
	maximumNumberOfLoadedTiles: 1000,
}));
```

## 平移

```js
// 计算偏移量tileset.boundingSphere,在3dtile加载完毕才能访问
function translation(tileset){
    // 获取模型边界球中心坐标,笛卡尔坐标转大地坐标
    var cartographic = Cesium.Cartographic.fromCartesian(tileset.boundingSphere.center)
    // console.log(cartographic)
    // 模型在地表中心位置
    var surface = Cesium.Cartesian3.fromRadians(cartographic.longitude,cartographic.latitude,0)
    // 目标位置
    var target = Cesium.Cartesian3.fromRadians(cartographic.longitude,cartographic.latitude,-20)
    // 平移变换
    var offset = Cesium.Cartesian3.subtract(target,surface,new Cesium.Cartesian3())
    // 应用变换
    tileset.modelMatrix = Cesium.Matrix4.fromTranslation(offset)
}
```

## 旋转

Cesium中模型旋转是绕着XYZ坐标轴的，实现模型绕着中心旋转较为麻烦，需要先将模型平移到地球球心，经过旋转变换后再平移到原来位置。

```js
// 旋转tileset,连续变换需要用左乘
function rotate(tileset){
    // 获取模型中心位置的笛卡尔坐标
    var center = tileset.boundingSphere.center;
    console.log("平移前:",center)
    // 将模型平移到球心
    var backto_matrix = Cesium.Matrix4.fromTranslation(center)
    var moveto_vec = Cesium.Cartesian3.multiplyByScalar(center,-1,new Cesium.Cartesian3())
    var moveto_matrix = Cesium.Matrix4.fromTranslation(moveto_vec)

    // 定义旋转方向
    var angleX = Cesium.Math.toRadians(0)
    var angleY = Cesium.Math.toRadians(0)
    var angleZ = Cesium.Math.toRadians(0)

    // console.log(trans1)
    // 将模型旋转
    var m1 = Cesium.Matrix4.fromRotation(Cesium.Matrix3.fromRotationX(angleX))
    var m2 = Cesium.Matrix4.fromRotation(Cesium.Matrix3.fromRotationY(angleY))
    var m3 = Cesium.Matrix4.fromRotation(Cesium.Matrix3.fromRotationZ(angleZ))
    //  定义变换矩阵
    var m = moveto_matrix
    m = Cesium.Matrix4.multiply(m1,m,new Cesium.Matrix4())
    m = Cesium.Matrix4.multiply(m2,m,new Cesium.Matrix4())
    m = Cesium.Matrix4.multiply(m3,m,new Cesium.Matrix4())
    // 再将模型平移到原来位置
    // console.log(m)
    m = Cesium.Matrix4.multiply(backto_matrix,m,new Cesium.Matrix4()) // 应用平移
    tileset.modelMatrix = m
}
```

## 缩放（后面补充）