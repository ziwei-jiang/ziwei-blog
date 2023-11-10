---
title: "Extended Superquadric"
date: 2022-02-07T03:14:03-05:00
weight: 5
# aliases: ["/first"]
tags: ["Computer Vision"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: false
hidemeta: false
comments: true
description: "A brief introduction to extended superquadrics."
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: true
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: false
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
math: true
cover:
    image: "/posts/superquadric/cover.png" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: true # when using page bundles set this to true
    hidden: false # only hide on current single page
    hiddenInList: false
    hiddenInSingle: false

---
Superquadric modeling was my first project in grad school, back when there were not as many 3D modeling techniques available as there are today. Using parametric models to describe 3D shapes has several advantages. First, this is a robust and efficient way to represent and recover 3D models for applications like large-scale building modeling. Second, the parametric model provides a natural way to describe shapes as primitives for applications such as point cloud segmentation and robot grasping.
We used a variation called extended superquadric by [Lin and Kambhamettu (2001)](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=fed4a22266c2da42c12b2c8cbd2512e6735531f0) for the project. 
It involves some concepts in computational graphics, one without such background might need some time to grasp the idea. So I hope this post could be helpful as introductory material summarizing key points about the extended superquadric.


# Superquadrics

Superquadric was introduced by Alan Barr in 1981. It is defined as a family of shapes, including superellipsoid and supertoroids. In most of the existing literature, "superquadrics" refer to superellipsoids since those are the most representative shapes in applications. 
The superquadric is defined by the following equation. 
$$\left(\left|\frac{x}{a_1}\right|^{\frac{2}{\epsilon_2}}+ \left|\frac{y}{a_2}\right|^\frac{2}{\epsilon_2}\right)^\frac{\epsilon_2}{\epsilon_1}+\left|\frac{z}{a_3}\right|^\frac{2}{\epsilon_1}=1$$
Where the $a_1$, $a_2$, and $a_3$ are the scale factor in the $x$, $y$, and $z$ directions, $\epsilon_1$ and $\epsilon_2$ control the squareness in the vertical axis and x-y plane, respectively. This is also known as the **inside-outside function** since it shows the relationship between any point and the surface. The set of superquadrics is shown in the figure. 
{{< figure align=center src="/posts/superquadric/superquadrics_photos.png" width="100%" title="Superquadrics">}}
The best way to understand the superquadric equation intuitively is to express it as a spherical product of two superellipses. 

The **superellipse**, also known as Lame curves, named after French mathematician Gabriel Lame, was discovered long before the popularization of superquadrics. The superellipse is defined by the following equation.
$$\left(\frac{x}{a_1}\right)^\frac{2}{\epsilon} +\left(\frac{y}{a_2}\right)^\frac{2}{\epsilon} =1$$

Similar to superquadrics, $a_1$ and $a_2$ are the scale factors and $\epsilon$ is the squareness of the shape.
{{< figure align=center src="/posts/superquadric/superellipse.png" width="60%" title="Superellipse">}}

## Parametric Form of Superquadrics

The spherical product is defined as an operation of two 2D curves. For the 2D curve $h(\omega)$ and $m(\eta)$, the spherical product is given as follows.
$$    h(\omega) = \begin{bmatrix}h_1(\omega) \\\ h_2(\omega) \end{bmatrix},\quad \omega_0 \leq \omega \leq \omega_1 $$
$$    m(\eta) = \begin{bmatrix}m_1(\eta) \\\ m_2(\eta) \end{bmatrix},\quad \eta_0 \leq \eta \leq \eta_1 $$
$$    x(\eta,\omega)= h(\omega) \otimes m(\eta) =
    \begin{bmatrix} 
    m_1(\eta) h_1(\omega)\\\
    m_1(\eta) h_2(\omega)\\\
    m_2(\eta)
    \end{bmatrix}$$



The most common example of a spherical product is that for a sphere for
which the spherical product involves a circle in the x-y plane and a half circle perpendicular to it.
We use $\phi$ for elevation and $\theta$ for azimuth.

$$m(\phi) = \begin{bmatrix}\cos\phi\\\ \sin\phi \end{bmatrix} ,\quad-\frac{\pi}{2}\leq\phi\leq \frac{\pi}{2}$$
$$h(\theta) = \begin{bmatrix}\cos\theta \\\ \sin\theta \end{bmatrix},\quad-\pi\leq \theta\leq\pi$$
$$r(\phi,\theta) = m(\phi)\otimes h(\theta) = \begin{bmatrix}x\\\y\\\z\end{bmatrix} = 
\begin{bmatrix}
\cos\phi\cos\theta\\\
\cos\phi\sin\theta\\\
\sin\phi
\end{bmatrix}$$

The superquadric is formed by the spherical product of two superellipses.
$$m(\phi) = \begin{bmatrix}sign(\cos\phi)\left|\cos\phi\right|^{\epsilon_1} \\\ a_3 sign(\sin\phi) \left|\sin\phi\right|^{\epsilon_1} \end{bmatrix},\quad-\frac{\pi}{2}\leq\phi\leq \frac{\pi}{2}$$
$$h(\theta) = \begin{bmatrix}a_1 sign(\cos\theta)\left|\cos\theta\right|^{\epsilon_2} \\\ a_2 sign(\sin\theta)\left|\sin\theta\right|^{\epsilon_2} \end{bmatrix},\quad-\pi\leq\theta\leq \pi $$
$$r(\phi,\theta) = m(\phi)\otimes h(\theta) = \begin{bmatrix}x\\\y\\\z\end{bmatrix} = \begin{bmatrix} a_1 sign(\cos\phi\cos\theta)\left|\cos\phi\right|^{\epsilon_1} \left|\cos\theta\right|^{\epsilon_2}  \\\
a_2 sign(\cos\phi\sin\theta)\left|\cos\phi\right|^{\epsilon_1} \left|\sin\theta\right|^{\epsilon_2} \\\
a_3 sign(\sin\phi) \left|\sin\phi\right|^{\epsilon_1}
\end{bmatrix}$$
This is the **parametric form** of the superquadric, which is useful for rendering the shape.
We can think of $h(\theta)$ as the curve on the x-y plane and it swept vertically according to the scale $m_1(\phi)$ and vertical sweeping motion $m_2(\phi)$. Similarly, it can auto be thought of as the curve perpendicular to the x-y plane sweeping around the z-axis according to $h(\theta)$ as shown in the following figure.
{{< figure align=center src="/posts/superquadric/spherical_product.gif" width="60%" title="Spherical Product">}}


## Derivation of Inside-outside Function

We can derive the inside-outside function from the parametric function above. 
First square both side of equation, and get 
$$\begin{bmatrix}x^2\\\y^2\\\z^2\end{bmatrix} = \begin{bmatrix} a_1^2 \left|\cos\phi\right|^{2\epsilon_1} \left|\cos\theta\right|^{2\epsilon_2}  \\\
a_2^2 \left|\cos\phi\right|^{2\epsilon_1} \left|\sin\theta\right|^{2\epsilon_2} \\\
a_3^2 \left|\sin\phi\right|^{2\epsilon_2}
\end{bmatrix}.$$

Then move the scaler terms to the LHS
$$\begin{bmatrix}(\frac{x}{a_1})^2\\\\(\frac{y}{a_2})^2\\\\(\frac{z}{a_3})^2\end{bmatrix} = \begin{bmatrix}  \left|\cos\phi\right|^{2\epsilon_1} \left|\cos\theta\right|^{2\epsilon_2}  \\\
 \left|\cos\phi\right|^{2\epsilon_1} \left|\sin\theta\right|^{2\epsilon_2} \\\
 \left|\sin\phi\right|^{2\epsilon_1}
\end{bmatrix}$$

Then raise the first two rows to the power of $\frac{1}{\epsilon_2}$ then sum the two rows.
$$\left(\frac{x}{a_1}\right)^\frac{2}{\epsilon_2}+\left(\frac{y}{a_2}\right)^\frac{2}{\epsilon_2} = \left|\cos\phi\right|^\frac{2\epsilon_1}{\epsilon_2}$$
Raise the equation to the power of $\frac{\epsilon_2}{\epsilon_1}$, get:
 $$\left(\left(\frac{x}{a_1}\right)^\frac{2}{\epsilon_2}+\left(\frac{y}{a_2}\right)^\frac{2}{\epsilon_2}\right)^\frac{\epsilon_2}{\epsilon_1} = \left|\cos\phi\right|^2$$

Then raise the third row to the power of $\frac{1}{\epsilon_1}$.
$$  \left(\frac{z}{a_3}\right)^\frac{2}{\epsilon_1}= \left|\sin\phi\right|^{2}$$

Finally, add the above two equations to get the inside-outside function. 
$$\left(\left(\frac{x}{a_1}\right)^\frac{2}{\epsilon_2}+\left(\frac{y}{a_2}\right)^\frac{2}{\epsilon_2}\right)^\frac{\epsilon_2}{\epsilon_1}  + \left(\frac{z}{a_3}\right)^\frac{2}{\epsilon_1} = 1$$

# Extended Superquadrics

The limitation of superquadrics lies in its symmetry property, which can be seen from its parametric form. To address this limitation, Zhou introduced the concept of an extended superquadric. Recall that the shape of a superquadric is determined by its exponents $\epsilon_1$ and $\epsilon_2$. We can create an asymmetric for the superquadric by breaking it into different sections with different exponents. The extended superquadric achieves this by using a function as the exponent instead of a constant. Zhou proposed using a Bézier curve as the exponent in his paper.

## Bézier Curve

There is a wonderful tutorial about the [**Bézier curve**](https://pomax.github.io/bézierinfo/) by Pomax. It provides a comprehensive introduction and interactive tools for the Bézier curve. Here I will briefly discuss the parts that are relevant to extended superquadrics.
The following polynomial specifies the Bézier Curve of degree n function.
$$ f(t) = \sum_{i=0}^n P_{i}B_i^n(t)$$
$B_i^n(t) = {n\choose i} t^i(1-t)^{n-i}$ is the [**Bernstein polynomial**](https://mathworld.wolfram.com/BernsteinPolynomial.html) of degree $n$. The degree of the polynomial is determined by the number of control points used. Each control point is specified by its $(x,y)$ coordinates in the $(x,y)$-plane. 
For an $n^{th}$ degree Bézier curve, there are a total of $n+1$ control points.

For each value of $0\leq t\leq 1$, the Bernstein polynomial adds up to 1, $\sum_i^n B_i^n(t) = 1$. So the Bézier curve can be interpreted as a weighted sum of the control points $P_i$, where the weights vary as $t$ ranges from zero to one. This is the key property that is used to control the shape of the extended superquadric. 

## Bézier Curves As Used in Extended Superquadrics

The extended superquadric is defined by the following inside-outside function
$$    \left(\left|\frac{x}{a_1}\right|^{\frac{2}{f_2(\theta)}}+ \left|\frac{y}{a_2}\right|^\frac{2}{f_2(\theta)}\right)^\frac{f_2(\theta)}{f_1(\phi)}+\left|\frac{z}{a_3}\right|^\frac{2}{f_1(\phi)}=1. $$

The two Bézier curves used are given by
$$ f_1(\phi) = \sum_{i=0}^nP_{1,i}{n\choose i} t^i (1-t)^{n-i},\qquad t = \frac{\phi + \frac{\pi}{2}}{\pi} ,$$
$$ f_2(\theta) = \sum_{i=0}^nP_{2,i}{n\choose i} t^i (1-t)^{n-i},\qquad t = \frac{\theta + \pi}{2\pi} ,$$
where 
$$ \phi = \arctan\frac{z}{\sqrt{x^2+y^2}}, $$
$$ \theta = \arctan(\frac{y}{x}). $$

The range of values for the azimuth angle $\theta$ is from $-\pi$ to $\pi$, while the range of values for the elevation angle $\phi$ is from $-\frac{\pi}{2}$ to $\frac{\pi}{2}$. To satisfy the definition of a Bézier curve, the parameter $t$ for both curves is normalized to the range between zero and one.

## Control the Shapes with Bézier Curve

In the previous section, we talked about how the value of a Bézier curve at any given $t$ is determined by a weighted combination of control points. If the control points are identical, the extended superquadric simplifies to an ordinary superquadric, indicating that the extended version is a more generalized form of the superquadric. The shape of the superquadric is determined by the first and last control points at t=0 and t=1, where they are assigned a weight of 1. As the weights depend on $t$, and the control points remain constant, employing more control points results in increased flexibility and variability in the superquadric. 

{{< figure align=center src="/posts/superquadric/two_pts.png" width="70%" title="Bézier curve with two control points">}} 

When there are only two control points, we can create an asymmetric shape by assigning different values to each control point. However, once we have determined the shape of the curve at t=0 and t=1, there is no freedom to vary the shape in other regions of the curve.


{{< figure align=center src="/posts/superquadric/three_pts.png" width="70%" title="Bézier curve with three control points">}}

When there are three control points, we can vary the shape of the curve by adjusting the value of the third control point. The position and weights of the first and last control points still determine the shape of the curve at t=0 and t=1, but the middle control point allows us to adjust the shape of the superquadric between those points.

{{< figure align=center src="/posts/superquadric/ten_pts.png" width="70%" title="Bézier curve with ten control points">}}

Similarly, when there are more control points, such as ten, we can create a more finely detailed variation of the curve's shape. The position and weights of the first and last control points will still determine the overall shape of the superquadric at t=0 and t=1, but the additional control points allow for more fine adjustments to the shape in the regions between them. The more control points there are, the greater the degree of control we have over the curve's shape.


| ![ex-superquadric](/posts/superquadric/ex1.png) | ![ex-superquadric-control](/posts/superquadric/ex1_control.png) |
|:---:|:---:|

An instance of an extended superquadric is presented where all control points are assigned a value of 1, except for one control point of $f_1$. Consequently, the resulting shape resembles a sphere with alterations on the upper portion of the superquadric.
The smoothness property of Bézier curves means that modifying a single control point also affects the surrounding area. One approach to address this issue is to use a piecewise Bézier curve, which consists of multiple Bézier curves with shared endpoints. Using a piecewise Bézier curve makes it possible to create local deformations without affecting the neighboring regions.


| ![ex-superquadric](/posts/superquadric/ex2.png) | ![ex-superquadric-control](/posts/superquadric/ex2_control.png) |
|:---:|:---:|

The following images show the deformation of the superquadric by using a piecewise Bézier curve.

| ![ex-superquadric](/posts/superquadric/ex3.gif) | ![ex-superquadric-control](/posts/superquadric/ex3_control.gif) |
|:---:|:---:|

| ![ex-superquadric](/posts/superquadric/ex4.gif) | ![ex-superquadric-control](/posts/superquadric/ex4_control.gif) |
|:---:|:---:|


## Example: BB8

Here is an example of an extended superquadric model. An untextured BB8 model that is composed of 6 extended superquadrics.

{{< figure align=center src="/posts/superquadric/bb8.gif" width="60%" title="A 3D model of BB-8 made by extended superquadrics">}}



# Conclusion

This post provides a brief introduction to the superquadric and extended superquadric. For a more in-depth study of the topic, please refer to [Segmentation and Recovery of Superquadrics](https://link.springer.com/book/10.1007/978-94-015-9456-1). The book discusses various aspects of superquadrics, including fitting them to data, segmenting complex shapes, and exploring different variations of superquadrics.

superquadrics can still be a useful method in certain cases, particularly for shape abstraction. Recent works such as [Superquadrics Revisited: Learning 3D Shape Parsing beyond Cuboids](https://openaccess.thecvf.com/content_CVPR_2019/papers/Paschalidou_Superquadrics_Revisited_Learning_3D_Shape_Parsing_Beyond_Cuboids_CVPR_2019_paper.pdf) and [Robust and Accurate Superquadric Recovery: a Probabilistic Approach](https://openaccess.thecvf.com/content/CVPR2022/papers/Liu_Robust_and_Accurate_Superquadric_Recovery_A_Probabilistic_Approach_CVPR_2022_paper.pdf) have demonstrated the applicability of superquadrics in this context.