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

当你看一副地图的时候，实际上，你是在看一个三维物体的二维展示（此处的三维物体指的是地球）。因为我们创建地图的时候，本质上来说丢失了地球的一个维度，没有任何一张地图能够完美的呈现地球。所有的地图都有一些变形。这些变形取决于我们所选择的投影方式（投影是一种将地球三维表面展示在二维平面上的方法）。在这一章节中，我们将讨论什么是投影，为什么说投影很重要，我们如何在Openlayers中使用投影。我们也会涉猎一些基础的图形学原理，学习这些将会帮助你更容易的掌握好Openlayers。

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

>Map projections No maps of the earth are truly perfect representations; all maps have some distortion. The reason for this is because they are attempting to represent a three dimensional object (an ellipsoid: the earth) in two dimensions (a plane: the map itself).
A projection is a representation of the entire, or parts of a, surface of a three dimensional sphere on a two dimensional plane (or other type of geometry).
Why on earth are Projections used? Every map has some sort of projection—it is an inherent attribute of maps. Imagine unpeeling an orange and then flattening the peel out. Some kind of distortion will occur, and if you try to fully fit the peel into a square or rectangle (like a flat, two dimensional map), you'd have a very hard time.
To get the peel to fit perfectly onto a flat square or rectangle, you could try to stretch out certain parts of the peel or cut some pieces of the peel off and rearrange them. The same sort of idea applies while trying to create a map.
There are literately an infinite amount of possible map projections; an unlimited number of ways to represent a three dimensional surface in two dimensions, but none of them are totally distortion free.
So, if there are so many different map projections, how do we decide on what one to use? Is there a best one? The answer is no. The 'best' projection to use depends on the context in which you use your map, what you're looking at, and what characteristics you wish to preserve.
Projection characteristics As a two dimensional representation is not without distortion, each projection makes a tradeoff between some characteristics. And as we lose a dimension when projecting the earth onto a map, we must make some sort of tradeoff between the characteristics we want to preserve. There are numerous characteristics, but for now let's focus on three of them.
Area Area refers to the size of features on the map. Projections that preserve area are known as equal area projections (also known as equiareal, equivalent, or homolographic). A projection preserves area if, for example, an inch measured at different places on the map covers the same area. Because area remains the same, angles, scales, and shapes are distorted. This is what an equal area projected map may look like:

dafad

>Scale Scale is the ratio of the map's distance to the actual distance (e.g., one centimeter on the map may be equal to one hundred actual meters). All map projections show scale incorrectly at some areas throughout the map; no map can show the same scale throughout the map. There are parts of the map, however, where scale remains correct—the placement of these locations mitigates scale errors elsewhere. The deformation of scale also depends on the area being mapped. Projections are referred to as equidistant if they contain true scale between a point and every other point on the map. Shape Maps that preserve shape are known as conformal or orthomorphic. Shape means that relative angles to all points on a map are correct. Most maps that show the entire earth are conformal, such as the Mercator projection (used by Google Earth and other common web maps). Depending on the specific projection, areas throughout the map are generally distorted but may be correct in certain places. A map that is conformal cannot also be  equal-area. The maps we've been using so far have been conformal. Other characteristics Projections have numerous other characteristics, such as bearing, distance, and direction. The key concept to take away here is that all projections preserve some characteristics  at the expense of others. For instance, a map that preserves shape cannot completely preserve area.
There is no 'perfect' map projection. The usefulness of a projection depends on the context the map is being used in. A particular projection may excel for a certain task, e.g. navigation, but can be a poor choice for other purposes.

adsfd

>Types of projections Projections are a way to represent three dimensions with two dimension surface. Projections are projected onto some geometric surface, three of the most common ones being a plane, cone, or cylinder.
Imagine a cylinder being wrapped around the earth, with the center of the cylinder's circumference touching the equator. Now, the earth is projected onto the surface of this cylinder, and if you cut the cylinder from top to bottom vertically and unwrap it, and lay it flat, you'd have a regular cylindrical projection:

adsfd


>

# 总结
