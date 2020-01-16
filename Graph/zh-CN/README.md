# 项目手册

本手册主要介绍本项目的创建的基础以及如何使用本项目完成计算机图形的操作。

## 1. 设计的基础

一般地，在数学中使用一个向量 $x=(x_0,x_1,\cdots,x_{n-1}) \in \mathbb{R}^n$ 来描述空间中的一个**点**（即 ●）；使用向量的减法表示一个**有向线段** $c = a - b$，即 $c$ 是由点 $b$ 指向点 $a$ 的有向线段（这里并不严谨）。考虑到计算机的离散特性，我们很难获取连续的“点”，故而，我们转向使用小方块 ■ 表示“点”（即图像的像素点）。这些 ■ 以不同的方式进行组合排列出不同的图案，比如线段、矩形框、椭圆、圆锥等。

我们先研究二维平面的点。

## 2. 画布：Canvas

这些点总是需要一个“承载”容器，在数学中，一般称其为坐标系或者参考系。笛卡尔坐标系为大家所熟知，在三维空间中一般被分为左手系和右手系。由于本项目讨论的坐标系是基于 tkinter 的 Canvas 模块，而 Canvas 的坐标系可以看作是左手系的二维版本，即：水平向右表示 $x$ 轴的正方向，竖直向下表示 $y$ 轴的正方向。

因为“点动成线，线动成面，面动成体”，所以我们可以使用 ■ 在 Canvas 画出各种各样的二维图像元素。为了定制统一化的图形元素，我们指定有向线段 $\text{direction}=(x_0,y_0,x_1,y_1)$ 为全部图形元素的基本属性，其中 $(x_0,y_0)$ 表示起点，$(x_1,y_1)$ 表示终点。

可以运行如下代码看几个常见的图形元素：

```python
def test_meta():
    from tkinter import Tk
    root = Tk()
    self = Meta(root, line_width=10, width=250, height=400, background='white')
    direction = 20, 20, 220, 300
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

图1 显示了红色的椭圆、绿色的扇形、蓝色的矩形、黑色的线段。
