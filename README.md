# 关于matrix3d的持续学习

**[2d的matrix传送门](https://zhuanlan.zhihu.com/p/74279787)**

**[本文章的demo](https://ztachi.github.io/learn-matrix3d/src/)**

**perspective**：允许作者做出的子元素具有的三维变换看起来好像他们生活在一个共同的三维空间。**perspective-origin**属性提供对应用透视的原点的控制，有效地改变“消失点”的位置。
[**perspective**为绘制表面(电脑屏幕)到眼睛的距离]

**transform-style**：允许3D转换的元素和它们的3D转化的后代都有一个共同的三维空间，使三维物体的层次结构的施工。

**backface-visibility**：通过三维变换翻转元素使得其反面对于观看者可见，在某些情况下，最好在这种情况下隐藏元素，这可以使用此属性的hidden值。

<font color=red>注意：虽然transform属性的某些值允许在三维坐标系中转换元素，但元素本身不是三维对象。相反，它们存在于二维平面（平坦表面）上并且没有深度。</font>

# 透视
**透视**和**透视原点**的属性可用于通过使元素更高的Z轴（更接近观看者）向深度的感觉添加到场景出现较大，而那些更远离于显得更小。缩放与d /（d - Z）成比例，其中**d为透视值**，是从绘图平面到观察者眼睛的假定位置的距离。  

<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/perspective_distance.png"></p>

_<p align="center">显示缩放的图表取决于透视属性和Z位置。在顶部图中，Z是d的一半。为了使原始圆（实心轮廓）看起来出现在Z（虚线圆圈），圆圈按比例放大两倍，产生浅蓝色圆圈。在底部的图表中，圆圈按比例缩小三分之一，使其显示在原始位置的后面。</p>_  

通常，观察者眼睛的假定位置以绘图为中心。如果需要，可以移动此位置 - 例如，如果网页包含应共享公共视角的多个图形 - 通过设置**perspective-origin**。  

<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/perspective_origin.png"></p>

_<p align="center">该图显示了向上移动透视原点的效果。</p>_

# 如何进行3D变换
要在我们网页的二维平面内构建3D效果，我们脑海中就要脱离二维的概念，想像屏幕里面的元素脱离屏幕正向你眼睛不断靠近或者不断远离。当然我们看不到这个元素本身，而只能看到处于三维空间的他在屏幕上的投影（也就是上一节那两张图所表示的）。

用css代码第一步就是设置一个透视——**perspective**。

假设我HTML结构如下：
```
<div class="container">
  <div class="transformed">
    <div class="child"></div>
  </div>
</div>
 ```
只要在 **.container** 加上 **perspective** ，在页面上的这个div与你眼睛之间就形成了第三个维度(此外还可以在需要3D透视的元素上单独设置 **transform:perspective(...px)** 为此元素独有的透视原点位置)，你看到的页面上的 **.container** 图形，就只是三维空间中的 **.container** 在二维平面的投影。可以通过 **translateZ** 来将元素移近或者移远你的眼睛。这里大家或许发现了一个问题，那就是如果 **translateZ** 的值大于了 **perspective** 会发生什么呢？这时候，由于元素已经移动到你 **眼睛后面** 去了，所以如果你的元素未在垂直距离上有倾斜(沿X或Y旋转)当然是看不到了。

现在我来看下实际效果：
```
.container {
    perspective: 500px;
    border: 1px solid black;
}

.transformed {
    transform: rotateY(50deg);
    background-color: blue;
}

.child {
    transform: rotateY(-40deg);
    background-color: lime;
}
```

<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/s1.png"></p>

_<p align="center">由于最外层设置了 **perspective**，所以当蓝色块沿Y轴顺时针旋转50°的时候，在二维世界的投影就变现为近大远小。</p>_


在这个例子里面，我在旋转元素 **.transformed** 里面还加入了一个子元素，让他也旋转， **.transformed** 的旋转很有3D立体感，但是他的子元素却没有，为什么？

因为 **perspective** 创建的三维空间只对第一级子元素 **(.transformed)** 有效。要想子元素 **(.transformed)** 的子元素 **(.child)** 也有立体感，有两种方式。

第一种就是在 **.transformed** 上面也加一个 **perspective** ，但是这样就有个问题，因为我 **.transformed** 已经做了旋转了，所以对于 **.transformed** 创建的 **独立的三维空间** ，朝向就有所不同，所以对于我们观察者来说的投影就有所不同，这或许不是我们想达到的效果。

第二种就是在 **.transformed** 上面加入 **transform-style: preserve-3d** 属性(使所有子元素都表现为3D特性,默认为 **transform-style: flat** 子元素为2D的特性)。加上这个属性之后，他下边的第一级子元素 **(.child)** 就与他共享一个 **(.container所创建的)** 三维空间了。代码、结果结果如下所示：

```
.container {
    perspective: 500px;
    border: 1px solid black;
}

.transformed {
    transform-style: preserve-3d;
    transform: rotateY(50deg);
    background-color: blue;
}

.child {
    transform: rotateY(-40deg);
    background-color: lime;
}
```

<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/s2.png"></p>

_<p align="center">本节代码见demo2</p>_

# 背面可见性
父级设置 **transform-style: preserve-3d** 之后，其所有子元素都具有了3D特性，此时允许设置 **backface-visibility** 来控制元素的背面是否可见。 **backface-visibility** 有两个值 **visible | hidden** ，默认为 **visible**。
<p align="center"><img src="https://ztachi.github.io/learn-matrix3d/src/noteImages/s3.png"></p>
_<p align="center">有了这个属性，我们就能很轻易的实现正反面扑克牌、正面朝外的立方体等效果（见demo3）</p>_
