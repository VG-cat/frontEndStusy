#前导知识

## 浏览器内核

浏览器内核就是渲染引擎，核心部分
负责解析网页代码，并且渲染页面

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-26_11-20-57.png)


标签+内容 =元素 

## html

<!DOCTYPE   HTML>   表明h5版本


- ### meta

  表示那些不能由其他 HTML 元相关（meta-related）元素表示的元数据信息。

src ==source

- ### a

  target："_self"
      _black
      _top          //结合ifarme使用，最上层
      _parent   //结合ifarme使用，父级

页面内锚点效果
href属性指向id名    

```
<h2  id='2'></h2>
<a href ='#2'></a>`

```

- iframe
  利用该元素可以实现在一个HTML页面中插入另一个HTML页面


###SEO

搜索引擎优化
title  
keywords
description

<meta name="keywords" content="关键词">
<meta name="description" content="描述">

font-size：0，来增加排名优化，但不影响页面

## css

### 伪类选择器
选择某一特定状态的元素

![Snipaste_2024-03-28_16-51-07](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-28_16-51-07.png)

![Snipaste_2024-03-28_20-57-27](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-28_20-57-27.png)

![Snipaste_2024-03-28_21-18-37](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-28_21-18-37.png)![Snipaste_2024-03-28_21-31-18](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-28_21-31-18.png)

选择器权重
div{
	color:red   !important 
}
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-29_14-49-11.png)

### 伪元素
使用css创建特定的元素，为inline级元素
在父级标签内创建标签

一定要加content:''
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-29_14-20-43.png)



![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-29_16-02-34.png)


###隐藏元素
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-29_19-16-49.png)


### 向上传递（塌陷）
margin会出现向上传递的效果，
1、父元素可以增加border去除传递效果或
3、、父元素可以设置padding-top或
2、触发bfc,设置overflow为auto

垂直反向上的两个margin，可能会合并为一个margin，这种现象叫折叠，取两者较大的值
水平方向永远不会

line-height === height  会居中

### 盒子阴影
box-shadow：offset-x,offset-y,blur,color,   //相对与xy轴的偏移，延申半径，模糊半径px   颜色 
text-shadow：offset-x,offset-y,blur,color,   //相对与xy轴的偏移，模糊半径px   颜色 


### 盒子级别
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-03-30_21-48-08.png)

### 浮动

浮动会脱离标准流，覆盖标签，不占空间，但文字会移动旁边

清除浮动：
#### 1、额外标签清楚法
父元素里面，在末尾加一个块级元素
，给这个块级元素添加属性，clear：both


#### 2、单伪元素清除法
.clearfix::after{
	content:''
	display:block
	clear:both
	height:0
	visiablity:hidden
}

#### 3、双伪元素清除法
.clear::before,  //解决外边距塌陷
.clearfix::after{
	content:''
	display:table	
}
.clearfix::after{	     
	clear:both

}

#### 4、父元素添加
overfloat：hidden

#### 5、父元素添加
高度

### 定位
#### 相对定位relative

相对与自己之前的位置移动
占有原来的位置
{
	left:10
	top:10
}
#### 绝对定位absolute

找已经定位的父级作为参考物进行定位
父级元素均没有定位，以浏览器窗口作为定位
改变显示模式特点，为行内块

#### 固定定位fixed

相对与浏览器窗口
不占原来位置
#### 子绝对父相对


### 特殊注意

span,a等行内元素，可以显示样式，但是不占空间
设置宽高，直接不生效
padding，margin可以将span等撑起来，但是不占空间

浏览器遇到行内和行内块标签按文字处理，默认文字是按基线对齐
vertical-align : middle，改变文字垂直方向对齐方式
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-01_15-13-35.png)
标准流《 浮动 《 定位

### 绘制三角形

书写一个盒子，h=w=0
盒子四个方向添加四个边，设置四个颜色
保留一个方向，其他方向为透明


### 精灵图
场景：将多张小图片合成一张大图片，这张大图片称为精灵图
优点：减少服务器发送次数，提高页面速度
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-01_15-35-58.png)

将图片设置为背景图片，移动背景图片
{
	bgcolor:
	bgimage:url('/')
	bgposition: 0  0
}

### 过渡

让属性慢慢变化
过渡属性+过渡时长
{
	transition : width  1s
}

### 平面转换

#### 平移
百分比相对于自己尺寸
{
	transform:translate(10%,10px)
}

translateX（）|| translateY（）

#### 旋转
{
	transform：rotate（30deg）
}

转换原点
transform-origin：1px 2px   //right bottom

#### 缩放

{
	transform：scale(2) //2倍
}

#### 多重转换

{
	transform：translate（） rotate（）
}

#### 渐变背景

background-Image :linear-gradient(transparent,black)


### 空间转换

#### 位移

{
	transform：translate3d(1,2,3) 
}

translateX（）|| translateY（）|| translateZ（）

透视
加给父级元素，建议800-1200px，人眼里屏幕的距离，只能实现近大远小
perspective：200px

#### 旋转
{
	transform：rotateZ(29deg)  //沿着Z轴旋转
}

rotate3d(x,y,z,deg)  //自定义轴旋转

#### 缩放

transform:scale3d(0.5,2,4);   //水平缩小一半，垂直放大一倍，厚度放大四倍

#### 3d空间

transform-style:preserve-3d
让子元素处于真正的3d空间



### 动画
定义两个状态

```
		@keyframes a1 {
            from {
                width: 200px;
            }

            to {
                width: 600px;
            }
        }

        div{
            width: 200px;
            height: 400px;
            background: pink;
            animation: a2  10s;
        }
```
```
定义多个动画时长
 		@keyframes a2 {
            0%{
                width: 200px;
            }

            25%{
                width: 300px;
            }
            50%{
                width: 600px;
            }
            75%{
                width: 200px;
            }
            100%{
                width: 600px;
            }
        }
```


animation: name duration timing-function delay iteration-count direction fill-mode;

iteration-count ------infinite  无限循环
direction ----------alternate  动画方向为往复运动，循环次数大于1才可以，偶数次为返回动画


fill-mode
forwards：结束状态为最终的状态600
backwards: ：结束状态为开始的状态200

timing-function速度曲线
step（）
linear  线性

动画暂停
two:hover{
   animation-play-state: paused;
}


补间动画
逐帧动画 ----精灵图运动


## 移动端页面

<meta name="viewport" content="width=device-width, initial-scale=1.0"> 

百分比布局，流式布局：宽度自适应（100%），高度固定

### Flex布局  弹性布局

可以避免脱标，更简单灵活，被提倡

父元素添加display：flex，子元素可以自动拉伸

父元素为弹性容器，子元素为弹性盒子，子元素默认沿着主轴排列

justify-content：center /space-around   元素沿主轴对齐方式   父级定义
align-items:center     //元素沿 侧轴的对齐方式     父级

align-self:center      //添加到指定元素，单独调整某一元素   子级 

flex:2   //子级的弹性比，占父元素剩余尺寸的份数    子级


flex垂直排列，调整主轴方向
flex-direction：column 

弹性盒子换行，不再压缩盒子大小
flex-warp：wrap

行是对齐方式
align-content：space-around

min-width,min-height


### 移动适配

#### rem

rem的单位是相对于HTML标签的字号计算结果
1rem = 1HTML字号大小  = 一般定义  1/10  视口宽度

px/36

html{
	font-size:20px
}

媒体查询，根据不同是媒体属性，设置差异化的样式
  @media (width:300px) {
            html{
                font-size: 30px;
            }
  }

flexible.js
框架解决上述问题，媒体查询需要写过多设备型号的问题


#### vw/vh

相对于视口计算
viewport width
viewport height

1vw = 1/100视口宽度  

两者不能混用
px/3.6




## 响应式布局

### 媒体查询

#### css写法
根据设备不同，设置差异化的样式

min-width
max-width

@media(max-width :1200px ){
	//小于等于1200px时
	div{
		bgcolor:red
	}
} 

避免样式层叠性，min要从小到大写，max要从大到小写
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-07_09-42-06.png)

#### link引入

<link rel="stylesheet" href="style.css"   media="(min-width:992px)">

### Bootstrap框架
是推特开发的前端ui框架，提供大量css样式，HTML，js，用于构建响应式页面

#### 栅格系统

类名：container定义容器


![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-07_10-05-57.png)

![Snipaste_2024-04-07_10-06-55](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-07_10-06-55.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-07_10-25-25.png)

## less
less是css的预处理器，文件后缀.less
浏览器不识别less文件

### 嵌套

&表示自身
```
.father{
    height: 200px;
    .son{
        height: 100px;

        &:hover{
            height: 20px;
        }
    }
}


```
### 变量

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-02_20-21-02.png)


### 导入文件

@import  ‘路径’


### 导出文件

插件改配置文件
或在文件第一行加入注释
//  out:  ./abc/

### 禁止导出
// out:  false

## Scss/Sass

sass3.0之前，文件后缀为.sass，3.0以后为.scss

### 变量

$a:88 55

css规则块内定义，则为局部变量

### 嵌套

同less
