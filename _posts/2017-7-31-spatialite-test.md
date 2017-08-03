---
layout: post
title: spatialite实践（一）spatialite资源汇总及基础用法
categories: Tools
description: 一次关于spatialite的实践
keywords: spatialite
---

# 背景
近期项目涉及到了使用patialite进行空间查询的内容，具体需求是：`使用高德地图的定位功能实时获取用户位置信息，然后到spatialite数据库中查询距离当前位置最近的建筑物面，标绘在地图上，并返回该建筑物的位置信息。`前几篇博客中提到的纠偏实践也是与此相关。

# 参考资料
[spatialite资源列表](http://www.gaia-gis.it/spatialite-2.3/)
`该网址包括我们开发过程中会使用到的大部分资料，包括文档、源码、编译后软件等等`

[spatialite cook](http://www.gaia-gis.it/gaia-sins/spatialite-cookbook/index.html)
`spatialite cook，可以理解为spatialite的入门指南。外国人很喜欢把入门指南说成某某某 cook，将学习编程的过程比作做饭，著名的开源js地图软件Openlayers也有与之配套的《Openlayers cook》`

[spatialite gui](http://www.gaia-gis.it/gaia-sins/windows-bin-amd64/)
`spatialite的管理界面软件，提供了便捷的导入shp等格式的矢量文件功能，同时也提供了可视化的数据增删改查功能。`

[spatialite for android](https://www.gaia-gis.it/fossil/libspatialite/wiki?name=spatialite-android-tutorial)
`适用andorid的spatialite使用指南。`

[spatialite android lib使用指南](http://blog.csdn.net/gispace/article/details/8155503)
`一篇中文的spatialite for android 使用指南，包含了三方包的编译等，很详细，也提供了很多的资源下载。`

# Android Studio使用spatialite for android
## 1·引入三方包
在如下图所示的红框部分，创建或者拷贝相应的文件夹及文件
![import lib](/images/posts/tools/spatialite-test/1.png)

## 2·在程序中获得项目工程assets下的资源
使用官方提供的`ActivityHelper`和`AssetHelper`两个类，按照如下所示的代码，即可在android程序中访问assets文件夹下的数据库文件。`MapConst`类为自定义的常量类，`MapConst.DB_NAME`代表`.splite`后缀的数据文件的名字，也就是我们的数据库文件。
```java
  private void initSqlLite(){
      try {
          AssetHelper.CopyAsset(this, ActivityHelper.getPath(this, false), MapConst.DB_NAME);
      } catch (IOException e) {
          ActivityHelper.showAlert(this,MapConst.ERROR_COPY_FAILED);
      }
      try {
          String dbFile = ActivityHelper.getDataBase(this,
                  MapConst.DB_NAME);
          Class.forName("jsqlite.JDBCDriver").newInstance();
          db = new jsqlite.Database();
          db.open(dbFile, jsqlite.Constants.SQLITE_OPEN_READONLY);
      } catch (IllegalAccessException e) {
          e.printStackTrace();
      } catch (InstantiationException e) {
          e.printStackTrace();
      } catch (ClassNotFoundException e) {
          e.printStackTrace();
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
```
## 3·spatialite for android使用示例（以查询某点周围x米范围内的点为例）
sql语句的写法与常规sql并无多大区别，但是要注意一些空间函数的使用，会使你的查询语句变的很简洁。
```java
  private void queryLocation(Double x,Double y){
      StringBuilder sqlBuilder=new StringBuilder("SELECT PK_UID as id, X as x, Y as y, AsText(Geometry) as geometry,");
      sqlBuilder.append("ST_Distance(Transform(MakePoint(f.x,f.y,4326),3857),");
      sqlBuilder.append("Transform(MakePoint(");
      sqlBuilder.append(x);
      sqlBuilder.append(",");
      sqlBuilder.append(y);
      sqlBuilder.append(",4326),3857)) as dis FROM fwm as f where dis<");
      sqlBuilder.append(MapConst.BUILD_BUFFER_DISTANCE);
      sqlBuilder.append(" order by dis");
      String querySql=sqlBuilder.toString();
      Log.i("sql statement:",querySql);

      List<BuildInfo> buildList=new ArrayList<BuildInfo>();
      try {
          Stmt stmt = db.prepare(querySql);
          while (stmt.step()) {
              BuildInfo build=new BuildInfo();
              build.setId(stmt.column_string(0));
              build.setX(Double.valueOf(stmt.column_string(1)));
              build.setY(Double.valueOf(stmt.column_string(2)));
              build.setGeometry(stmt.column_string(3));
              build.setDist(Double.valueOf(stmt.column_string(4)));
              buildList.add(build);
          }
          stmt.close();
          if(buildList.size()>0){
              BuildInfo build=(BuildInfo)buildList.get(0);
              if(build.getDist()>MapConst.BUILD_BUFFER_DISTANCE){
                  Log.i("info:","No build in "+MapConst.BUILD_BUFFER_DISTANCE+"meter buffer of current position.");
                  return;
              }else{
                  Log.i("info:",build.getId()+"-"+build.getDist());
              }
              System.out.println(build.getGeometry());
              MultiPolygon multiPolygon =transformWKT.getMULTIPOLYGONWktToJson(build.getGeometry(),4326);
              List<Double[]> rings=multiPolygon.getRings().get(0);
              LatLng[] path=new LatLng[rings.size()];
              int idx=0;
              for(Double[] lnglat : rings){
                  path[idx]=new LatLng(lnglat[1],lnglat[0]);
                  idx++;
              }
              if(marker!=null){
                  marker.remove();
                  marker.destroy();
                  marker=null;
              }
              if(polygon!=null){
                  polygon.remove();
                  polygon=null;
              }
              polygon=mapPainter.drawPolygon(path);
              marker=mapPainter.drawPoint(build.getX(),build.getY());
          }else{
              Log.i("info:","No build in "+MapConst.BUILD_BUFFER_DISTANCE+"meter buffer of current position.");
          }
      } catch (Exception e) {
          e.printStackTrace();
      }
    }
```

# 结束语
以上语句中并没有进行索引优化，这部分内容可以在`spatialite cook`中找到相应的介绍，这里不再赘述。近期项目赶得紧，空闲时间不多，介绍的不是很系统，只能等日后抽空再做补充。

网络上关于spatialite的中文资料不多，同志们还是多加学习英语，争取能无障碍阅读吧。
