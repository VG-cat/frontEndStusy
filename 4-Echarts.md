## 简介

ECharts低层依赖矢量图形库ZRender，可以流畅运行在PC和移动设备上面，高度个性定制化图表

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-08_15-00-44.png)

使用：
1、准备dom容器
2、初始化echarts实例
3、指定图表的配置项和数据
4、指定的配置项和数据显示图表

## 图表配置信息

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-10_09-19-22.png)

## 项目适配

rem单位    +  flexible.js     +    flex布局

## 边框图片

当div大小不一样，但是样式一样时，使用边框图片调整边框样式

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-08_20-43-03.png)


## 等比缩放

//4、当我们浏览器缩放的时候，图表也等比例缩放

window. addEventListener( "resize" , function(){
	//让我们的图表调用resize这个方法
	myChart.resize();
})();
