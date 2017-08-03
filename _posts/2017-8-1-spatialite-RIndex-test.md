---
layout: post
title: spatialite实践（二）spatialite R*Index 查询实践
categories: Tools
description: spatialite R*Index 查询实践
keywords: spatialite R*Index 查询优化
---
# 背景
空间查询是一种及其复杂的查询，无论是相交、邻接，还是包含关系，都需要进行复杂的图形位置计算，这对于cpu的消耗是极大地，查询的效率也是极低的。因此，我们需要借助于索引来进行查询相关的优化。

# 实践过程
### 场景描述
在spatialite中，根据手机实时定位数据，查询距离定位位置200m之内的建筑物面。下面将就是用索引与不使用索引的查询时间进行比较。

### 不使用索引
是用以下查询语句进行查询，与常规查询语句相似，但多了一些空间函数的使用。
```sql
SELECT ROWID, "PK_UID" as id, "X" as x, "Y" as y, "Geometry" as geometry,
	ST_Distance(Transform(MakePoint(f.x,f.y,4326),3857),
    	Transform(MakePoint(116.944,36.663,4326),3857)) as dis
FROM "fwm" as f
where dis<200
order by dis
```
>MakePoint(f.x,f.y,4326) 是将一对x、y坐标包装秤坐标系为espg 4326的点数据结构。

>Transform(MakePoint(f.x,f.y,4326),3857)是将点的坐标由4326转化为3857。我们知道，4326坐标系的单位是经纬度，3857坐标系的经纬度为米，要计算距离当然是米来的更直观。

>ST_Distance(Transform(MakePoint(f.x,f.y,4326),3857),
    Transform(MakePoint(116.944,36.663,4326),3857))是计算
两点之间的距离

这样整个语句就很明白了，下面我们来看看执行效果。

![](/images/posts/tools/spatialite-RIndex-test/1.png)

我们发现，以上的sql语句进行了全表扫描，共扫描了三万余行数据，效率非常低。共耗时4.5s，这对一次查询而言可以说是灾难了。

![](/images/posts/tools/spatialite-RIndex-test/2.png)

### 使用R*Index索引
使用以下语句进行查询，比较前一个sql语句，多了对索引表的查询。
```sql
SELECT ROWID, "PK_UID" as id, "Geometry" as geometry,X as x,Y as y,
	ST_Distance(Transform(MakePoint(x,y,4326),3857),Transform(MakePoint(116.944,36.663,4326),3857)) as dis
	FROM "fwm" as f
where f.id in(
	SELECT pkid
    	FROM idx_fwd_geometry
    	WHERE pkid MATCH RTreeDistWithin(116.944,36.663,0.01)
)
order by dis
```
spatialite若想利用空间索引，必须使用以下一句sql语句从索引表中查询，这与常规的sql查询是不同的。要注意，如果不查询索引表，即使在空间数据列上添加了空间索引，也不会对查询起到加速作用。
>SELECT pkid
    FROM idx_fwd_geometry
    WHERE pkid MATCH RTreeDistWithin(116.944,36.663,0.01) 这一句是利用空间索引表进行一次过滤，筛选出某点周围1km左右的点的id集合。注意，此处的0.01是1/100 degree，大约相当于1000m。

按照如上语句查询，仅耗时0.2s就查询完成。

![](/images/posts/tools/spatialite-RIndex-test/3.png)

### RIndex在海量数据时的表现
GIS行业总是与大数据量密切相关，不管是poi点数据、房屋面数据、人口数据还是交通路线，在一个市甚至一个省来讲，其体量都是巨大的。因此，GIS对数据库处理海量数据的能力有更高的要求。下面我们来测试一下50w数据量下spatialite的性能表现。

![](/images/posts/tools/spatialite-RIndex-test/4.png)

创建`BUILD_AREA`表，导入shp，用如下sql语句进行查询。
```sql
SELECT ROWID, "PK_UID" as id, "Geometry" as geometry,X as x,Y as y,
	ST_Distance(Transform(MakePoint(x,y,4326),3857),Transform(MakePoint(116.944,36.663,4326),3857)) as dis
	FROM BUILD_AREA as f
where f.pk_uid in(
	SELECT pkid
    FROM idx_BUILD_AREA_geometry
    WHERE pkid MATCH RTreeDistWithin(116.944,36.663,0.001)
)
order by dis
```
运行结果如下：

![](/images/posts/tools/spatialite-RIndex-test/5.png)

我们看到，经过空间索引过滤，即使是48w的数据量，我们进行某点周围一百米的查询仍可以做到0.2s返回。此处不得不感叹工程师们的鬼斧神工以及spatialite索引机制的玄妙。

# 总结
spatialite的空间查询优化是个需要耐心的工作，其中还有很多不明白的地方，这些语句多是根据`spatialite cook`总结的，其中仍有很多不明白的地方等待探索。

>注：最新版本的spatialite gui不能使用空间索引函数，原因不明，需要使用`spatialite cook`中推荐的旧版本的spatialite gui才能正常使用空间索引函数。

[spatialite gui旧版本下载地址](http://www.gaia-gis.it/spatialite-2.4.0-4/binaries.html)
