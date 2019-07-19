# 关于matrix3d的持续学习

**[demo](https://ztachi.github.io/learn-matrix3d/src/)**

**perspective**：允许作者做出的子元素具有的三维变换看起来好像他们生活在一个共同的三维空间。**perspective-origin**属性提供对应用透视的原点的控制，有效地改变“消失点”的位置。
（**perspective**为绘制表面到眼睛的距离）

**transform-style**：允许3D转换的元素和它们的3D转化的后代都有一个共同的三维空间，使三维物体的层次结构的施工。

**backface-visibility**：通过三维变换翻转元素使得其反面对于观看者可见，在某些情况下，最好在这种情况下隐藏元素，这可以使用此属性的hidden值。

注意：虽然transform属性的某些值允许在三维坐标系中转换元素，但元素本身不是三维对象。相反，它们存在于二维平面（平坦表面）上并且没有深度。

# 透视
**透视**和**透视原点**的属性可用于通过使元素更高的Z轴（更接近观看者）向深度的感觉添加到场景出现较大，而那些更远离于显得更小。缩放与d /（d - Z）成比例，其中**d为透视值**，是从绘图平面到观察者眼睛的假定位置的距离。  

<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/perspective_distance.png"></p>

_<p align="center">显示缩放的图表取决于透视属性和Z位置。在顶部图中，Z是d的一半。为了使原始圆（实心轮廓）看起来出现在Z（虚线圆圈），圆圈按比例放大两倍，产生浅蓝色圆圈。在底部的图表中，圆圈按比例缩小三分之一，使其显示在原始位置的后面。</p>_  

通常，观察者眼睛的假定位置以绘图为中心。如果需要，可以移动此位置 - 例如，如果网页包含应共享公共视角的多个图形 - 通过设置**perspective-origin**。  

<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/perspective_origin.png"></p>

_<p align="center">该图显示了向上移动透视原点的效果。</p>_
