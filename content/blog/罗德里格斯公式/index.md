---
title: '罗德里格斯公式'
date: '2025-06-20 18:37:12'
summary: 'A structured note on Rodrigues rotation formula, covering intuition, derivation, and implementation in Eigen.'
authors:
  - me
tags:
  - '图形学'
  - 'C++'
math: true
toc: true
---

本文主旨是便于理解公式的大致原理和使用过程，其中数学推导可能有诸多不严谨的地方，希望多多包涵以及批评指正
<!--more-->
## 起因
![alt text](/image/rod_image_0.png)
看到题目的第一反应，是按照类似平移的思路，将向量axis绕y轴与x轴旋转变换至与z轴对齐，其角度取$arctan(y/\sqrt{(x^2 * z^2)})$与$arctan(x/\sqrt{(y^2 * z^2)})$，然后进行绕z轴的旋转变换再把之前进行的变换进行反变换就行了。然而稍微想想就能发现这样的错误，在绕y轴旋转之后，x与z都会发生变化，第二次旋转变换的角度完全是错误的，所以无法正确得到答案。因此，需要使用**罗德里格斯公式(Rodrigues' rotation formula)**来解决这个问题。
## 什么是罗德里格斯公式
在三维旋转理论体系中，罗德里格旋转公式是在给定转轴和旋转角度后，旋转一个向量的有效算法。这个公式以欧林·罗德里格命名。罗德里格于1840年发表此公式。
$$ \mathbf {v} _{\mathrm {rot} }=\mathbf {v} \cos \theta +(\mathbf {k} \times \mathbf {v} )\sin \theta +\mathbf {k} (\mathbf {k} \cdot \mathbf {v} )(1-\cos \theta )
$$
上面的公式则是罗德里格斯公式的向量形式。在图形学中，通常还会采用罗德里格斯公式的变换矩阵形式：
$$
\mathbf{R}(n,\alpha) = cos(\alpha)\mathbf{I} + (1-cos(\alpha))\mathbf{n}\mathbf{n}^T + sin(\alpha)
\begin{bmatrix}
0&-n_z&n_y\\
n_z&0&-n_x\\
-n_y&n_x&0
\end{bmatrix}
$$
下面将对公式进行推导。
## 推导
![Rodrigues 旋转公式图解](/image/rod_image_1.png)
*图：Rodrigues 旋转公式图解，作者 Maschen，来源 [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Orthogonal_decomposition_unit_vector_rodrigues_rotation_formula.svg)，授权协议：CC0（公有领域）。*
设将要被旋转的向量为$\mathbf{v}$, 所绕的轴单位向量为$\mathbf{k}$, $\mathbf{v}$在$\mathbf{k}$方向上竖直分量记为$\mathbf{v_{||}}$,在与$\mathbf{k}$垂直的方向上的水平分量记为$\mathbf{v_{⊥}}$, 旋转的角度为$\theta$。
显然：
$$\mathbf{v} = \mathbf{v_{||}} + \mathbf{v_{⊥}}$$
当v旋转成$\mathbf{\hat{v}}$后，有
$$\mathbf{\hat{v}} = \mathbf{\hat{v_{||}}} + \mathbf{\hat{v_{⊥}}}$$
$\mathbf{v_{||}}$与$\mathbf{k}$平行，所以$\mathbf{\hat{v_{||}}}$在旋转后依然与$\mathbf{k}$平行, 即$\mathbf{\hat{v}}$的竖直分量方向与$\mathbf{k}$一致, 有
$$\mathbf{\hat{v_{||}}} = \mathbf{v_{||}} = (\mathbf{v}·\mathbf{k})\mathbf{k}  $$
$\mathbf{k}×\mathbf{v} = \mathbf{k}×(\mathbf{v_⊥} + \mathbf{v_{||}})$, 其中$\mathbf{k} × \mathbf{v_{||}} = 0$,因此$\mathbf{k} × \mathbf{v}= \mathbf{k} × \mathbf{v_{⊥}}$。
考虑水平方向的旋转，可知
$$\mathbf{v_{⊥}}cos\theta + (\mathbf{k}×\mathbf{v})sin\theta = \mathbf{\hat{v_{⊥}}}$$
于是
$$\mathbf{\hat{v}} = \mathbf{v_{⊥}}cos\theta + (\mathbf{k}×\mathbf{v})sin\theta + (\mathbf{v}·\mathbf{k})\mathbf{k}$$
又因为
$$\mathbf{v_{⊥}} = \mathbf{v} - \mathbf{v_{||}} = \mathbf{v} - (\mathbf{v}·\mathbf{k})\mathbf{k}  $$
所以
$$\mathbf{\hat{v}} = (\mathbf{v} - (\mathbf{v}·\mathbf{k})\mathbf{k})cos\theta + (\mathbf{k}×\mathbf{v})sin\theta + (\mathbf{v}·\mathbf{k})\mathbf{k}$$
整理后得到
$$ \mathbf {v} = \mathbf {v} \cos \theta +(\mathbf {k} \times \mathbf {v} )\sin \theta +\mathbf {k} (\mathbf {k} \cdot \mathbf {v} )(1-\cos \theta )
$$
也就是罗德里格斯公式的向量形式。
对于变换矩阵形式，我们先把$\mathbf {v}$提出来
$$\cos \theta +(\mathbf {k} ×\mathbf{v})/\mathbf{v}\sin \theta +\mathbf {k} (\mathbf {k} )(1-\cos \theta )$$
对于向量叉乘,可以看作一个向量的反对称矩阵乘另一个向量，于是有
$$\mathbf {k} ×\mathbf{v} = \begin{bmatrix}
    0&-n_z&n_y\\
    n_z&0&-n_x\\
    -n_y&n_x&0
\end{bmatrix}\mathbf{v}$$
令轴向量为$\mathbf {n}$,则有
$$\mathbf{I}\cos \theta + \begin{bmatrix}
    0&-n_z&n_y\\
    n_z&0&-n_x\\
    -n_y&n_x&0
\end{bmatrix} \sin \theta +\mathbf {n} \mathbf {n^T} (1-\cos \theta )$$
正是罗德里罗斯公式德变换矩阵形式。
体现在代码中
```cpp
//使用Eigen线性代数库
Eigen::Matrix4f get_rotation(Vector3f axis, float angle){
    //Rodrigues's Rotation Formula
    Eigen::Matrix4f r1, r2, r3;
    //Normalize vector
    float lenth = sqrt(axis[0] * axis[0] + axis[1] * axis[1] + axis[2] * axis[2]);
    if(lenth == 0)return Eigen::Matrix4f::Identity();
    axis[0] = axis[0] / lenth;
    axis[1] = axis[1] / lenth;
    axis[2] = axis[2] / lenth;
    //Horizontal component
    r1 << cos(angle), 0, 0, 0,
        0, cos(angle), 0, 0,
        0, 0, cos(angle), 0,
        0, 0, 0, 1;
    float t = 1 - cos(angle);
    //vertical component
    r2 << t * axis[0] * axis[0], t * axis[0] * axis[1], t * axis[0] * axis[2], 0,
        t * axis[1] * axis[0], t * axis[1] * axis[1], t * axis[1] * axis[2], 0,
        t * axis[2] * axis[0], t * axis[2] * axis[1], t * axis[2] * axis[2], 0,
        0, 0, 0, 0;
    t = sin(angle);
    r3 << 0, t * -axis[2], t * axis[1], 0,
        t * axis[2], 0, t * -axis[0], 0, 
        t * -axis[1], t * axis[0], 0, 0,
        0, 0, 0, 0;    
    return r1 + r2 + r3;
}
```
