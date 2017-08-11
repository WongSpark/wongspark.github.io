---
layout: post
title: Openlayers2（4）Projection
categories: Tools
description: Openlayers2（4）Projection
keywords: Openlayers Projection
---
# 背景
笔者使用Openlayers有两年左右的时间了，一些常用的功能反反复复的使用过数遍，对一些功能的实现也有简单的了解。但是，学问不能止于此，有些概念仍然要多多学习。我对Openlayers的入门算是从这本《Openlayers Beginner》开始的，如今也想着返璞归真，重新阅读一遍原书，并尝试简单翻译。习笔拙作，难入法眼，在此贻笑大方了。

# 译文
>When you look at a map, you are looking at a two dimensional representation of a three dimensional object (the earth). Because we are, essentially, 'losing' a dimension when we create a map, no map is a perfect representation of the earth. All maps have some distortion. The distortion depends on what projection (a method of representing the earth's surface on a two dimensional plane) you use. In this chapter, we'll talk more about what projections are, why they're important, and how we use them in OpenLayers. We'll also cover some other fundamental geographic principles that will help make it easier to better understand OpenLayers.

当你看一副地图的时候，实际上，你是在看一个三维物体的二维展示（此处的三维物体指的是地球）。因为我们创建地图的时候，本质上来说丢失了地球的一个维度，没有任何一张地图能够完美的呈现地球。所有的地图都有一些变形。这些变形取决于我们所选择的投影方式（投影是一种将地球三维表面展示在二维平面上的方法）。在这一章节中，我们将讨论什么是投影，为什么说投影很重要，我们如何在Openlayers中使用投影。我们也会涉猎一些基础的地理学原理，学习这些将会帮助你更容易的掌握好Openlayers。

>In this chapter, we will cover:

  >Concept of map projections 

 >Types of projections 

 >Longitude, latitude, and other geographic concepts 

 >OpenLayers projection class 

 >Transforming coordinates

>Let's get started!

在这一章节，我们将会涉及到：

·地图投影的概念

·投影的种类

·经纬度及其他地理学概念

·Openlayers projection 类

·坐标转换

我们开始吧！

>Map projections

> No maps of the earth are truly perfect representations; all maps have some distortion. The reason for this is because they are attempting to represent a three dimensional object (an ellipsoid: the earth) in two dimensions (a plane: the map itself).

>A projection is a representation of the entire, or parts of a, surface of a three dimensional sphere on a two dimensional plane (or other type of geometry).


地图投影

没有地图可以真实完美的展现地球；所有的地图都有一定的变形。产生变形的原因是：我们试着将一个三维的物体（一个椭球体：地球）展示为二维（一个平面：地图本身）。

投影是一种用来在二维平面上呈现整个或者部分三维空间物体表面的表现手法。

>Why on earth are Projections used?

>Every map has some sort of projection—it is an inherent attribute of maps. Imagine unpeeling an orange and then flattening the peel out. Some kind of distortion will occur, and if you try to fully fit the peel into a square or rectangle (like a flat, two dimensional map), you'd have a very hard time.

>To get the peel to fit perfectly onto a flat square or rectangle, you could try to stretch out certain parts of the peel or cut some pieces of the peel off and rearrange them. The same sort of idea applies while trying to create a map.

>There are literately an infinite amount of possible map projections; an unlimited number of ways to represent a three dimensional surface in two dimensions, but none of them are totally distortion free.

>So, if there are so many different map projections, how do we decide on what one to use? Is there a best one? The answer is no. The 'best' projection to use depends on the context in which you use your map, what you're looking at, and what characteristics you wish to preserve.

为什么使用投影？

每一幅地图都有一种投影，这是地图的固有属性。想象一下，剥开橘子皮，并将它展开。这时候橘子皮就会发生一些变形，如果你想让橘子皮完全符合正方形或者矩形的形状（比如一个平面的、二维的地图），你很难做到。

为了使橘子皮能够完美的契合到正方形或者矩形中，你可以试着撕下其中某些部分，或者剪下橘子皮的某一部分，重新排列组合。我们创建地图的时候应用的也是这种方法。

地图投影的方法有无数种，在一个二维平面上展示三维物体的手段也有无数种，但是没有一种是完全没有形变的。

所以，既然种类如此繁多的地图投影，我们应该如何决定使用哪一种投影呢？有没有一种投影是最好的投影？答案是否定的。‘最合适’的投影取决于你的地图的应用场景，你希望得到什么样的效果和你想保留地图的那些特性。




# 总结
