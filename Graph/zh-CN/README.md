# 项目手册

本手册主要介绍 [TensorAtom](https://github.com/TensorAtom)/[Graph](https://github.com/TensorAtom/Graph) 项目的创建的基础以及如何使用该项目完成计算机图形的相关操作。

可以直接使用 PyPI 的安装方式进行安装：

```sh
$ pip install graph-tensor
```

之后的使用需要载入：

```python
import graph_tensor
```

## 1. 设计的基础

一般地，在数学中使用一个向量 $x=(x_0,x_1,\cdots,x_{n-1}) \in \mathbb{R}^n$ 来描述空间中的一个**点**（即 ●）；使用向量的减法表示一个**有向线段** $c = a - b$，即 $c$ 是由点 $b$ 指向点 $a$ 的有向线段（这里并不严谨）。考虑到计算机的离散特性，我们很难获取连续的“点”，故而，我们转向使用小方块 ■ 表示“点”（即图像的像素点）。这些 ■ 以不同的方式进行组合排列出不同的图案，比如线段、矩形框、椭圆、圆锥等。

我们先研究二维平面的点。

## 2. 画布：Canvas

这些点总是需要一个“承载”容器，在数学中，一般称其为坐标系或者参考系。笛卡尔坐标系为大家所熟知，在三维空间中一般被分为左手系和右手系。由于本项目讨论的坐标系是基于 tkinter 的 Canvas 模块，而 Canvas 的坐标系可以看作是左手系的二维版本，即：水平向右表示 $x$ 轴的正方向，竖直向下表示 $y$ 轴的正方向。

因为“点动成线，线动成面，面动成体”，所以我们可以使用 ■ 在 Canvas 画出各种各样的二维图像元素。为了定制统一化的图形元素，我们指定有向线段 $\text{direction}=(x_0,y_0,x_1,y_1)$ 为全部图形元素的基本属性，其中 $(x_0,y_0)$ 表示起点，$(x_1,y_1)$ 表示终点。

可以运行如下代码看几个常见的图形元素：

```python
from graph_tensor.graph.atom import Meta


def test_meta(direction = (20, 20, 220, 300)):
    from tkinter import Tk
    root = Tk()
    self = Meta(root, line_width=10, width=250, height=400, background='white')
    self.draw_graph(direction, 'Rectangle', 'blue')
    self.draw_graph(direction, 'Oval', 'red')
    self.draw_graph(direction, 'Arc', 'green', 'pieslice')
    self.draw_graph(direction, 'Line', 'black')
    self.layout()
    root.mainloop()

test_meta()
```

显示的效果见图1：

![图1 常见的图形元素](../images/test_meta.png)

图1 显示了红色的椭圆、绿色的扇形、蓝色的矩形、黑色的线段。从图中可以观察到给定相同的 `direction`，画出的图形元素有着相同的中心。从本质上讲，`direction` 可以看作是椭圆、扇形、矩形以及线段的方向向量。只要给定 `direction`，那么它们之间的关系就被牢牢的锁定了。

这里的 `Meta` 没有直接提供画“多边形”的方法，其实画“多边形”可以由画“线段”组合得到。下面给出代码实现：

```python
from tkinter import Tk

def point2polygon(points):
    '''将点列转换为首尾相接的线段'''
    directions = []
    for start, end in zip(points, points[1:]):
        directions.append((*start, *end))
    directions.append((*points[-1], *points[0]))
    return directions

def draw_polygon(meta, *directions):
    '''画出多边形'''
    for direct in directions:
        meta.draw_graph(direct, 'Line', 'black', fill='red')

root = Tk()
self = Meta(root, line_width=5, width=250, height=400, background='white')

points = [
    (20, 20), (30, 50), (100, 250),
    (250, 340), (220, 230)
]
directions = point2polygon(points)
draw_polygon(self, *directions)
self.layout()
root.mainloop()
```

效果图见图2：

![图2 画出多边形](../images/meta_polygon.png)

您也可以使用 `self.create_polygon(points, fill='red')` 画出有内部填充的多边形。下面以“矩形”为例来说明这些图形元素的更多方法。

### 2.1 画虚线

可以使用如下代码画出虚线图形（需要指定参数 `dash`）：

```python
def test_meta_dash(direction = (20, 20, 220, 300)):
    from tkinter import Tk
    root = Tk()
    self = Meta(root, line_width=10, width=250, height=400, background='white')
    self.draw_graph(direction, 'Rectangle', 'blue', dash=5)
    self.layout()
    root.mainloop()

test_meta_dash()
```

效果见图3：

![图3 Meta 画虚线的示例](../images/meta_dash.png)