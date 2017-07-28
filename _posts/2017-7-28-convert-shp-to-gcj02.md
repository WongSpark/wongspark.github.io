---
layout: post
title: shp数据坐标系转换实践
categories: Tools
description: 一次关于转换shp数据坐标系的实践
keywords: shp，坐标转换
---

## 背景
关于shp数据的坐标转换问题，在上一篇博文中其实已经提过，但是语焉不详，估计难以看懂，所以特此补充一篇详细的转换过程博文，以供参考。

坐标转换过程中用到了‘使用ArcEngine移动Feature’的技术，受ArcEngine限制，该过程执行效率并不高，三万条数据大概处理了15分钟。此处提出一点建议：

`可将移动polygon的操作另起一个线程进行，这样可以避免与主线程（绘制界面）竞争资源，提高执行效率，也避免了界面卡住。`

## 实践过程
以下实践以polygon shp为例，个人认为polygon的坐标转换要比point和line难，所以以此为讲解素材。

* 在Arcgis中打开shp数据，并为shp数据添加x，y坐标属性（即添加图形要素的中心点坐标），具体操作如下：

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/1.png)

* 右键点击table of contents中的图层，选择open attribute table，再点击如图所示按钮，选择Add field

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/2.png)

* 弹出对话框按照如下填写，注意圈出部分，点击ok

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/3.png)

* 同样的操作添加Y字段，结果如下：

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/4.png)

* 右键点击X字段，选择Calculate Geometry

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/5.png)

* 注意下面圈出两项的一致性

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/6.png)

* 同理计算Y的坐标值

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/7.png)

* 结果如图

![OpenGrok Search and Browse](/images/posts/tools/convert-shp-to-gcj02/8.png)

* 接下来，使用ArcgisEngine开发包，解析shp并修改其数据，完成纠偏。代码如下：
```
private void movePolygon()
        {
            IMovePolygonFeedback m_MovePolygonFeedback = new MovePolygonFeedbackClass();
            IFeatureLayer pFeatureLayer = axMapControl1.get_Layer(0) as IFeatureLayer;
            IFeatureCursor pFeatureCursor;
            pFeatureCursor = pFeatureLayer.FeatureClass.Search(null, false);
            IFeature pFeature = pFeatureCursor.NextFeature();

            IWorkspaceEdit workspaceEdit;
            IWorkspace workspace;
            IDataset dataset = pFeatureLayer.FeatureClass as IDataset;
            workspace = dataset.Workspace;
            workspaceEdit = workspace as IWorkspaceEdit;
            //开始编辑（会开启一个session，消耗资源）
            workspaceEdit.StartEditing(true);
            workspaceEdit.StartEditOperation();

            while (pFeature != null)
            {
                int xidx = pFeature.Fields.FindField("X");//需与shp属性表中的字段对应起来
                int yidx = pFeature.Fields.FindField("Y");
                double x = pFeature.get_Value(xidx);
                double y = pFeature.get_Value(yidx);
                Console.WriteLine("原坐标：" + x + "_____" + y);
                IPoint preLocation = new ESRI.ArcGIS.Geometry.PointClass();
                preLocation.PutCoords(x, y);
                m_MovePolygonFeedback.Start(pFeature.Shape as IPolygon, preLocation);

                double[] convertLocation = projConvert.wgs84togcj02(x, y);
                Console.WriteLine("纠偏后坐标：" + convertLocation[0] + "_____" + convertLocation[1]);

                //修改xy字段值，改为纠偏后值
                pFeature.set_Value(xidx,convertLocation[0]);
                pFeature.set_Value(yidx, convertLocation[1]);

                //移动对象到当前位置
                IPoint desLocation = new ESRI.ArcGIS.Geometry.PointClass();
                desLocation.PutCoords(convertLocation[0], convertLocation[1]);
                m_MovePolygonFeedback.MoveTo(desLocation);

                IGeometry resultGeometry = m_MovePolygonFeedback.Stop() as IGeometry;
                pFeature.Shape = resultGeometry;
                pFeature.Store();
                pFeature = pFeatureCursor.NextFeature();
            }
            //结束编辑
            workspaceEdit.StopEditOperation();
            workspaceEdit.StopEditing(true);

            m_MovePolygonFeedback = null;
            MessageBox.Show("完成");   
        }
```

* 结果如图所示：
![OpenGrok Search and Browse](/images/posts/tools/wgs84-correct-to-gcj02-1.png)

## 后话
此处仅仅是移动了shp数据的要素位置和修改了要素的坐标数据，并未在Arcgis中对其定义的投影坐标系进行修正，但经过检验，不影响数据的叠加使用，在修改投影坐标系方面如有需要，请自行搜索，网络上教程很多。

有时间会补充上openlayers加载高德地图叠加修正数据的例子。
