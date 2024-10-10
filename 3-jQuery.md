##简介
js类库，用于简化dom操作，封装了很多方法

## 选择器

\$()
\$('p')  //选中p标签，得到一个jQuery对象
\$('.mp')  //选中mp类名

## jQuery对象

jQuery对象和dom对象不一样不能混写

\$('p').css('bgcolor','pink')
dom.style.bgcolor = 'pink'


qobj = $(dom)   //将dom对象转化为jQuery对象

## 事件绑定

\$('p').click(function (){
	console.log(this)   //this为当前的dom对象
})

## 链式编程		

\$('p').change().focus().click()


## 事件方法

### 入口函数

window.onload(function(){})

$(window).on('load',function)   //资源全部加载完毕

//下面两个先执行，dom完成后执行
$(document).ready(function(){})

$(function(){})

### 内容操作

\$('p').html()  //获取标签或设置标签
\$('p').text()   //不支持链式编程

### 过滤方法

\$('p').first().html()   //获取第一个
\$('p').last().html()
\$('p').eq().html()   //获取第n个，从0开始

### 样式操作

设置行内样式

\$('p').css(‘bgcolor’）  //取值
//赋值
\$('p').css({
	bgcolor:red,
	font-size:20px
})

### 属性操作

\$('a').attr('src')
\$('a').attr('src',"aaa.com")

\$('a').removeAttr('src')

### 查找方法

.parent()
.children()
.siblings()
.find('div')


### 类名操作

.addClass('active')
.removeClass('active')
.hasClass('active')
.toggleClass('active')    //切换，有则删除，没有则添加


## 事件进阶

### 注册事件

某些方法不能通过上面方法处理事件绑定

\$('div').on('input',function(){})

### 移除指定事件

\$('div').off('input'）

### 移除所有事件

\$('div').off(）

### 注册一次性事件

\$('div').one('input',function(){})

## 触发事件

\$('div').click(）

\$('div').trigger('click'）

\$(window).click(）



## 节点操作

### 插入

在父元素结尾
.append("<img src=''>")

在父元素开头
.prepend('<>')

在兄弟元素前插入
.before('<>')

在兄弟节点后插入
.after('<>')

### 移动

移动到父元素结尾
\$('p').append($('div'))

移动到父元素开头
.prepend($('div'))

在兄弟元素前插入
.before($('div'))

在兄弟节点后插入
.after($('div'))

### 删除

删除本元素的节点，谁调用删除谁，删除自己

\$('p').remove()     jQuery对象才可以用该方法

\$('p').parent().remove()     jQuery对象才可以用该方法

### 克隆

$('div').clone()   //返回jQuery对象
$('div').clone(true)   //克隆对象和事件


## 转为DOM对象

$('div').get(0)  //获取第0个div

$('div')[0]


## 获取尺寸

内边距：padding

外边距：margin

![](.\images\img\Snipaste_2024-04-07_21-12-01.png)

## 事件参数

.click(function(e){
	
})

阻止冒泡，阻止默认行为等和js一样

## 事件委托

\$('父元素选择器').on(事件名，后代选择器，function(){})

## jQuery动画

.offset()  获取距离页面上方和左方的位置
.position()  获取定位父元素的上方和左方位置
.scrollTop()
.scrollLeft() 

### 显示隐藏动画
.show(1000)   //元素显示，时长1000毫秒
.hide()   //隐藏
.toggle()   //切换状态

### 淡入淡出动画
.fadeIn(1000)   //淡入动画
.fadeOut()  //淡出动画
.fadeToggle()  //切换效果

### 展开收起动画

.slideDown()  //展开动画
.slideUp()
.slideToggle()  //切换


### 动画停止

.stop()    //停止动画
.stop(true)  //清空动画队列，在动画当前状态停止
.stop(true,true)   //清空动画队列，并直接到动画的最终状态


### 自定义动画

.animate({
	scrollTop:20px,
	scrollLeft:20px
},1000)


### 回调函数

在动画执行完后，立即执行回调函数

属性，执行时间，回调函数

.animate({
	scrollTop:20px,
	scrollLeft:20px
},1000，function(){})

### 延迟方法

动画执行前设置一定的延时，单位ms

$('p').delay(1000).动画方法（)

## jQuery插件


### 轮播图插件  slick

![](.\images\img\Snipaste_2024-04-08_10-04-00.png)

![](.\images\img\Snipaste_2024-04-08_10-04-37.png)

### 懒加载插件 lazyload

图片在看到以后加载，不是随页面立即加载

![](.\images\img\Snipaste_2024-04-08_10-16-28.png)

### 全屏滚动插件  fullpage
营销广告页面

![](.\images\img\Snipaste_2024-04-08_10-38-51.png)

![Snipaste_2024-04-08_10-39-11](.\images\img\Snipaste_2024-04-08_10-39-11.png)

### 日期选择器插件  datepicker


不同浏览器统一样式

![](.\images\img\Snipaste_2024-04-08_11-08-38.png)

![Snipaste_2024-04-08_11-09-00](.\images\img\Snipaste_2024-04-08_11-09-00.png)

### 表单验证插件 validate

![](.\images\img\Snipaste_2024-04-08_13-40-13.png)

![](.\images\img\Snipaste_2024-04-08_13-41-18.png)

![](.\images\img\Snipaste_2024-04-08_13-48-29.png)

## 表单序列化

\$('form').serialize()
返回
name='222'&age=22

## 插件的机制

jQuery.fn.extend({
	sayHellow:function(){
	
	}
})

往jQuery原型上面添加函数，所有jQuery对象均可以使用

