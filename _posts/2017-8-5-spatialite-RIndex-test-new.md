---
layout: post
title: spatialite实践（三）spatialite3.2版本后关于R*Index的修改
categories: Tools
description: spatialite R*Index 查询实践
keywords: spatialite R*Index 查询优化
---
# 背景
上一阵子笔者学习了spatialite，对其索引机制进行了略微深入一些的了解。也解决了上一篇学习笔记中提到的关于`1.3版本以上的spatialite gui不能使用索引函数`的问题。

# 相关资料
先放上一篇`spatialite4.0`版本升级日志，里面提到了有关原有索引函数的弊端以及新版本推荐的空间索引的使用方式。

[switching-to-4](https://www.gaia-gis.it/fossil/libspatialite/wiki?name=switching-to-4.0)

[SpatialIndex-Update](http://www.gaia-gis.it/spatialite-3.0.0-BETA/SpatialIndex-Update.pdf)

其中有一段这样的描述

>Test #2: we'll just DELETE some rows; then we'll VACUUM the DB in order to reclaim any unused storage space.
And finally we'll perform the same identical query using the R*Tree Spatial Index: but this time
we'll get a wrong result set. A row is obviously missing.
Why ? really simple to explain … because the R*Tree is now severely corrupted.

译文：我们删除一些数据，然后为了取回不再使用的空余存储，我们将清理数据库。接着，我们执行执行和刚才一样的使用R*Tree空间索引的查询语句，但是这次，我们得到了错误的执行结果。一行数据丢失了。为什么？简单来说，R*Tree现在被破坏了。

总结来说，如果我们对一张没有创建主键的表创建空间索引的，当这张表里的数据删除后，就会影响空间索引的使用。所以新版的spatialite不在推荐使用原有的空间索引函数，转而引入了一种新的空间索引查询语句。

>The latest versions of SpatiaLite (starting since version 3.0.0) introduced a new mechanism supporting the Spatial Index and based on VirtualSpatialIndex.

最新版本的spatialite 4.0 不再支持 RTreeDistthin等RTree开头的方法，转而使用最新的VirtualSpatialIndex interface。

语法如下：
```sql
SELECT ROWID
FROM SpatialIndex
WHERE f_table_name = <table_name>
      AND search_frame = <some_geometry>
```
实例如下：
```sql
SELECT ROWID    FROM SpatialIndex
WHERE f_table_name = 'fwd' AND f_geometry_column = 'Geometry' AND
      search_frame = BuildMbr(116.944,36.663, 116.945,36.664, 4326)
```

其中SpatialIndex是固定的，f_table_name对应你要查找的表名，f_geometry_column对应你要使用的列，search_frame对应用来过滤数据的图形。

# 总结
索引是个神奇的技术，位图索引、R树索引都是常见的索引技术，而我对此还没有深入实践，接下来有时间一定要尝试。
