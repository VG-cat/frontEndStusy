#  javaScript

## 简介

编程语言，人机交互，运行在客户端

## 输出

document.write()
console.log()
prompt('ggg')      	//弹出输入框

## 常量与变量

cosnt    常量
let,var 变量

## 数据类型

弱数据类型

数字型 ：整数，负数，小数
字符串类型， '  '   , "   ",    `  `
布尔类型
未定义类型   undefined
null
object

模板字符串： `你好${name}`    反引号加$

## 数据类型转换

### 隐式转换

1-'1'
1\*'1'
1/'1'
+'1'   会将字符串型转换未数字型
1+'1'  会将数字型转化为字符串型
    

### 显式转换

Number(),String()
parseInt()，只保留整数，不进行四舍五入       parseFloat()，保留全部

### 对象与字符串拼接
当你尝试将对象与字符串拼接，比如使用 `+` 操作符时，JavaScript 引擎会首先尝试将对象转换为原始值（通常是字符串）。这通常遵循以下步骤：

1. 如果操作数是 `null` 或 `undefined`，它们会被转换为字符串 `"null"` 或 `"undefined"`。
2. 如果操作数是一个对象，包括数组，JavaScript 引擎会调用对象的 `toString()` 方法，并将对象转换为其 `toString()` 方法返回的字符串值。
3. 其他类型的值（如数字、布尔值等）会被转换成它们的字符串表示形式。

## 比较运算符

 	==      只比较值
 	===		比较值和类型，全等
 	！==	   值和类型有一个不一样的就会返回true
 	
 	NaN 不等于任何值，包括他本身

## 无限循环

while（true）{}
for（；；）{}

## 数组操作 

arr.push('333')      //加到结尾
arr.unshift('333')   //加到开头

arr.pop()     //删除结尾最后一个
arr.shift()   //删除开头一个
arr.splice(下标，个数)    //删除指定数量

## 函数

### 具名函数
function fun(){}
fun()

### 函数表达式

let fun = function(a,b=2){}  //b为默认参数
fun()

### 匿名函数
function(){}

立即执行
（function(x,y){}）（1，2）

### 逻辑中断

aaa   ||  ''              //返回第一个真
qqq  &&  ''         //两个都是真，则返回最后一个真；两个有一个假，则返回第一个假

##对象

const  obj = {}
const obj = new Object()

属性名的引号默认省略

对象遍历      for(x in obj)      x为属性名

###  环境对象
函数中都有一个this，谁调用，谁就是this
```
ul.add(function(){
	this   //为ul
})
```

## 获取DOM元素

<div  data-spm='xxxxx'    data-id='sssss'></div>
const div = document.querySelector('div')    //获取第一个匹配的dom元素
const div = document.querySelectorAll('.box')    //获取全部元素    得到伪数组

const div =  getElementById，getElementsByTagName

. aaa {
	color:red
}

### 更改样式

//更改样式，会覆盖以前的样式
div.className = 'aaa'

//追加样式，不影响以前样式
div.classList.add('aaa')
移除样式
div.classList.remove('aaa')
切换样式，原来有就删掉，原来没有就添加
div.classList.toggle('aaa')
是否包含样式
div.classList.contains('sss')   //是否有样式sss,返回t/f

### 自定义属性
data-XXX                // 自定义属性以data开头   如 data-spm  
div.dataset.spm     //获取自定义属性
div.dataset.id

## 定时器

let  a = setInterval(fun,1000)    周期调用
clearInterval(a)      //关闭定时器

## 事件

### 事件对象

div.addEventListener（‘click’，function（event）{}）

event即为事件对象

里面的属性：
type: 事件类型
clientX：窗口左上角的位置
offsetY：相对与当前DOM位置
key:  按下键盘的值

### 事件绑定
div.addEventListener（‘click’，function（）{}）

div.onclick方式会出现覆盖事件问题，addEventListener则不会，会添加事件

div.click()    //调用点击事件

### 事件解绑

div.onclick = none

//匿名函数无法解绑
div.removeEventListener('click',functionA)

### 事件流
事件流：事件完整执行过程中的流动路径
事件捕获和事件冒泡两个阶段

dom.addEventListener('click',func,true)
true：表示事件捕获，从外往里找同名事件（click）
false：默认，表示事件冒泡，从里往外

阻止冒泡和捕获，阻止事件流动：
dom.addEventListener('click',function(event){
	event.stopPropagation()
})

阻止默认行为
event.preventDefault()

### 事件委托

事件委托是利用事件流的特性来解决开发需求的知识技巧，减少注册次数，提高性能

子元素的事件会冒泡到父元素身上，利用父元素进行处理，对父元素注册事件

```
 	<ul>
        <li>Lorem.</li>
        <li>Sed.</li>
        <li>Animi!</li>
        <li>Est.</li>
        <li>Corporis.</li>
    </ul>
    
     document.querySelector('ul').addEventListener('click',function(e){
        e.target.style.color = 'yellow'    //e.target点击的对象
        console.dir(e.target) //显示所有属性
    })
```


### 其他事件
####  页面加载

所有资源加载完毕后，执行
window.addEventListener('load',function(){})

//图片资源加载完成，执行
img.addEventLinstener('load',function(){})

//dom元素加载完成后，执行
document.addEventLinstener('DOMContentLoaded',fun)

#### 页面滚动事件

window.addEventListener('scroll',function(){})
div.scrollTop
div.scrollLeft    //获取div卷曲

document.documentElement.scrollTop    //获取页面上部卷曲像素，检测页面卷动长度，**可读写属性**

#### 页面缩放事件

window.addEventListener('resize',function(){}) 

#### 获取元素尺寸位置

div.clientWidth和div.clientHeight    //获取可见大小，包括padding，不包括margin和board和滚动条
div.offsetWidth和offsetHeight    //获取可视宽高，包括padding和board，隐藏盒子为0

div.offsetLeft   div.offsetTop   //获取距离最近定位父元素的距离 ，**只读属性**

div.getBoundingClientRect()  //返回对象，包括元素大小，相对于视口的`位置`
![](.\images\js\QQ截图20240315224943.png)


#### 页面平滑滚动

css里面加
html{
	scroll-behavior:smooth
}


#### 音频/视频事件

ontimeupdate   播放位置发生变化时触发
onloadeddata   播放当前帧已准备，下一帧的数据没准备完成时触发，可以理解为一打开页面触发

## 日期对象

const Date = new Date('2022-10-1 08:11:11')

### 常见方法
.getHours()
.toLocaleString()   //2022-01-09 22:22:22

### 时间戳
1970年1月1日到现在的毫秒数

用于倒计时效果

.getTime()   或
+new Date() 或
Date.now()

## 节点操作

### 节点查找
dom.parentNode   //最近父级元素，没有则返回none

dom.childNodes   //获取所有节点，包括文本
dom.children     //仅获取元素节点

dom.querySelecter(li:nth-child(2))    //获取第几个li
dom.nextElementSibling   //获取下一个兄弟
dom.previousElementSibling  //获取上一个兄弟

### 节点增加

cosnt li  = document.createElement('li')
father.appendChild(li)
father.insertBefore(li,某节点)  //插入某节点前

### 节点克隆
const li2 = li.cloneNode(true)    //true为深克隆，false为浅克隆

### 节点删除
父删子

fatherDOM.removeChild(li)

## M端事件（移动端）

touchstart    //接触时触发
touchmove  //滑动时触发
touchend    //离开时触发

## 其他

### 回调函数

函数A作为参数给函数B，函数A就是回调函数

### 伪类选择器

.calssName:clicked

### 属性选择器

input[value]{
	选取有value属性的input框，password没有value
}

input[type=password]{
	选取type属性为password的input框，单独定义
}

dom.querySelector('[type=password]'')




## BOM（浏览器对象模型）
![](.\images\js\Snipaste_2024-03-17_14-51-23.png)

### location对象
拆分保存了url地址的各个部分

- href
location.href = 'www.baidu.com'   //利用href进行页面跳转
- search
location.search    //获取地址？之后的参数部分，？name='333'
- hash
location.hash   //获取地址#后面的部分，#/my
- reload（）
location.reload(true)   //刷新页面，true表示强制刷新

### navigator
获取浏览器自身的相关信息

- useAgent
navigator.useAgent   //获取浏览器版本及平台信息 
```
    ! (function  {
    const userAgent = navigator. userAgent//验证是否为Android或iPhone
    const android = userAgent.match(/(Android); ? [\sV/]+([ \d.]+)?
    const iphone = userAgent.match( /( iPhone\soS)\s([ld_]+)/)//如果是Android或iPhone，则跳转至移动站点
    if ( android || iphone){
    	location.href = 'http: //m.itcast.cn '
    })()

```
!function(){}()   === ~function(){}()   === +function(){}()    ===  (function(){})()   立即执行函数

### history
和历史相关

- back()
后退一步
- forward()
前进一步
- go()
前进n布
go(-1),go(1)

## 本地存储
解决页面刷新数据丢失，存储到本地浏览器中

- localStorage
永久存储在本地电脑中，除非手动删除，页面关闭也会存在
同一浏览器数据共享，同域多页面也共享，以键值对的形式
```
localStorage.setItem(key,value)
localStorage.getItem(key)
localStorage.removeItem(key)
localStorage.clear(key) 
```

- sessionStorage
生命周期为关闭浏览器窗口，同一个页面下的数据可以共享，以键值对的形式存储
用法同上

复杂数据类型存储
```
obj = {
	a:1
	b:1
}
localStorage.setItem(key,Json.stringify(obj))
Json.parse(localStorage.getItem(key))
```
## 正则表达式

1. 定义规则
const  reg  = /规则/
2. 语法
/规则/.test(str)

/前端/.test('里面有前端吗？')   //返回  true||false
/前端/.exec('里面有前端吗？')   //返回信息数组   ||null

元字符
- 边界符
^开头           //   /^h$/   精确匹配，只有一个h，hh为false
$结尾
- 量词
\* 重复0-      //   /k+/
+重复1-
？重复0或1次
{n}重复n次
{n,m}   重复至少n次，最多m次
- 字符集
[0-9]匹配字符集合
[a-z]匹配a-z里的任意一个字符
\[^0-9]    //取反，除去0-9的任意一个字符
.      除换行符外的任意一个字符
- 预定义
\d === [a-z]
\D ===[^a-z]
\w ===[a-zA-Z0-9]
\W
\s ===匹配空格
\S ===除去空格的字符
- 修饰符
/q/i   //不区分大小写
/q/g  //全局匹配的字符
new = str.replace(/reg/g,'aa')    //字符串中满足规则的 替换为aa

# js进阶（ES6)

##作用域链
作用域链就是低层的变量查找机制
函数被执行时，优先查找函数作用域中的变量，找不到就逐级查找父级作用域

## 垃圾回收机制
自动进行

### 生命周期

内存分配：声明变量，函数，系统会自动分配内存
内存使用：使用变量，读写内存
内存回收：使用完毕，由垃圾回收器自动回收

全局变量在页面关闭时回收
局部变量在不使用时立即回收

内存泄漏：分配的内存无法释放

### 回收算法
引用计数法-----循环引用
标记清楚法-----全局查找，找不到就标记删·

##闭包
闭包= 内层函数+外层函数的变量
闭包的作用：封闭数据，调用私有变量，提供操作，外部函数也可以访问函数内部发变量

```
function outer(){
	const a =1;
	function inner(){
		console.log(a)
	}
	return inner
}

const a = outer()
```
闭包可以访问创建时父作用域中的变量，即使这些变量(a)在父函数(outer)执行完毕后仍然可以被访问。有内存泄漏的风险

## 变量提升

console,log(num)
var num = 10
使用var定义的变量，会将变量声明提升到当前相同作用域的最前面
变量值为undefine，只提升声明，不提升赋值

## 函数进阶

### 函数提升
同变量提升一样
```
fun()
function fun(){}
```
### 函数参数
动态参数 
function fun(){
	console.log(arguments) //只存在域数组里，伪数组，箭头函数里没有该方法
}
剩余参数
function fun(...arg){}

### 箭头函数this
箭头函数自身没有this，只会从作用域链，沿用上一层的this

改变this指向的方法
function fun(x,y){}
1、 fun.call(obj，1，3)  //this的指向改为obj，并传入参数1，2，返回结果

2、 fun.apply(obj,[1,2])  //仅参数传递不同 

3、 const newFun = fun.bind(obj)   //不会调用函数，返回一个新函数

## 解构赋值

###数值解构

批量赋值给变量
const [a,b,...arr] =[1,2,3]
交换变量      ;[a,b] = [b,a]

数组的forEach
arr.forEach((item,index)=>{})   //只遍历不返回
###对象解构

const {name} = {name:89}
const {name:newName} = {name:89}  //改名
function fun({data}){}

## 深入对象

### 构造函数
是一种特殊的函数，主要用来创建初始化对象，提取公共属性

function Pig(name,age,gender){
	this.name = name
	this.age = age
	this.gender = gender
}
const pipi = new Pig('pipi'，18，男)

### 创建对象

const obj = {}
const obj = new Object({})
const pipi = new Pig('pipi'，18，男)

### 成员

实例成员：实例对象的属性和方法
pipi.name
静态成员：构造函数的属性和方法
```
Pig.eye = 2
log(Pig.eye)   //2
log(pipi.eye) //undefine
```

### 方法

####Object对象
Object.keys({a:1})
Object.values({a:1})

Object.assign({a:1},{b:2})   //将后面对象的属性，拷贝到前面对象中去

Object.defineProperty(obj,"b",{
	value:"world",
	enumerable:false,
})  //添加属性

#### Array数组
arr.forEach()  //只遍历不返回
arr.map()
arr.filter()
arr.reduce(function(上一次值，当前值){}，初始值)   累计处理   
arr.every(（item）=> item === 2)   //每一个元素都是2，返回T/F
arr.some()  //部分是2  返回true
arr.sort()  ///对原数组进行排序
Array.form() //伪数组转真数组,可以传入set

#### Number
.toString()
.toFixed(2)  //保留两位小数，四舍五入

## symbol类型

主要解决属性名冲突的问题 

Symbol('1')  === Symbol('1')   //false

let a = Symbol(‘a’)

let obj = {}

obj[a] = 33   //属性名a具有唯一性，防止同名覆盖

## proxy

拦截处理操作,参数为对象和规则对象

```
const student = {
	userName:2
}

//将student对象进行拦截，之后使用拦截后的对象
const proxy = new Proxy(student,{
	//获取属性规则
	get:function(target,property){
		if(property in target){
			return target[property]
		}else{
			throw new Error('property+不存在')
		}
	}，
	//设置属性规则
	set:function(obj,prop,value){
		if(prop === 'userName'){
			if(Number.isIniget(value)){
				obj.userName = value
				return
			}else{
				throw new Error(’错误)
			}
		}
	}
})    

log(proxy.userName)
proxy.userName = 44
```



## Generator迭代器函数

```
function* go(str){
	log(1)
	let a = yield str+11  //执行到此处暂停,a为str11，yield 为执行过程
	log(2)
	let b = yield a   //a为next()传入的参数
	log(3)
	return b
}

let it = go('ss')
let a = it.next()   //执行到yield暂停，输出 1
log(a)  //{value:'ss11',done:false}

let b = it.next('dd')  //输出 dd,输出 2


for(let a of go()){
	log(a)   //循环 ，str11  a  b 
}
```

```

//循环切换   go().next()
function* go(){
	while(true){
		img.src = 'a.png'
		yield 1
		img.src = 'b.png'
		yield 2
	}
}
```

```
//异步发送请求
function* go(){
	const res = yield fct('/dev/get')
	json.parse(res)
}

function fct(url){
	request(url).then(res=>{
		it.next(res)
	})
}
const it = go()
```



## 原型

### 原型对象

由于通过构造函数创建，每次创建实例对象都会分配内存，造成内存的浪费

每一个构造函数都有一个protoType属性，指向另一个对象，所以我们称为原型对象

这个原型对象不会多次创建，可以把不变的函数和变量放在protoType对象上，所有的实例对象都共享方法和变量
达到节约内存的目的

function Pig(namer){
	this.name = name
	this.prototype.sing = function(){}
}或
Pig.prototype.sing = function(){this}

const pipi = Pig(2)
pipi.sing( )

this指向实例对象  

```
		Pig.prototype.py = 20
		console.log(pipi.py)   //20
		
        pipi.py = 30      //覆盖原属性，并在实例对象中添加属性
        console.log(pipi.py)   //30
        console.log(Pig.prototype.py)  //20
        console.log(Pig.py)   //undefine
```

### constructor
Pig.prototype ={
	constructor:Pig   //重新指向父构造函数 
	sing：function(){this}
}会出现找不到父的情况

### 对象原型

__proto__ === [[prototype]]
用来表名当前的实例对象指向哪一个原型对象
__proto__里面也有一个constructor，指向实例对象的构造函数

![](.\images\js\Snipaste_2024-03-22_10-32-58.png)

### 原型继承

function Person（） {
	this.eye = 2
}

Pig.prototype =new Person()  //Pig继承Person
Pig.prototype.constructor = Pig

### 原型链
基于原型对象的继承使得不同的构造函数的原型对象关联在一起，并且这种关联的关系是一种链状结构
是一种查找规则，一直查找到null为止


//对象封装式组件
```
function Modal(title){
	this.modalBox = document.createElement('div')
	this.modalBox.innerHtml = `<div class='y'>${title}<div>`
	
	this.protitype.open = function(){
		document.body.append(this.modalBox)
		this.modalBox.queryselector('.y').addEventListener('click',()=>{
			this.close()    //此处必须要用箭头函数，this要指向实例对象
		})
	}
	this.protitype.close = function(){
		document.remove(this.modalBox)
	}
}

new modal()
```

## 深浅拷贝
浅拷贝，拷贝地址，基本数据类型可以， 多层（>=1）复杂对象时还是会同时变
Object.assign(a,b)   //b复制到a
a = [...b]
a = b

深拷贝
1、函数递归，逐个解析出简单数据类型再进行赋值
2、lodash库的deepClone（），\_.deepClone
3、 newObj = Json.parse(Json.stringify(obj))

## 异常处理
```
function(){
	try{
		throw  new Error('错误')
	} catch(err){
		log(err.message)  //不中断程序
	} finally{
	 	//不论对错，一定执行
	}
	
}
```
## 防抖（debounce）

单位时间内，频繁触发事件，只执行最后一次
- lodash
```
停止移动后，500ms执行
div.addEventListener('mousemove',_.debounce(move,500))
```
- 手写原理
核心是settimeout
```
 		let timerId = null
        function move2(){
            if(timerId){
                clearTimeout(timerId)
            }
            timerId = setTimeout(()=>{
                div.innerHTML = i++
            },500)
        }
        div.addEventListener('mousemove',move2)
```

## 节流throttle
单位时间内，频繁触发事件，只执行一次

- lodash
```
500ms内只触发一次
div.addEventListener('mousemove',_.debounce(move,500))
```
- 手写原理
核心是setTimeout
```
		let timerId = null
        function move2(){
            if(timerId){
                return
            }
            timerId = setTimeout(()=>{
                div.innerHTML = i++
                timerId = null    //clearTimeout不能用，定时器开启期间无法关闭
            },500)
        }
        div.addEventListener('mousemove',move2)
```
![](.\images\js\Snipaste_2024-03-22_23-02-38.png)

# 事件循环

### 进程与线程

进程：每一个应用占用一块内存空间，这块内存空间可以理解为进程，至少分配一个进程，进程之间相互独立，通信需要经过双方同意
线程：线程负责运行代码，一个进程至少包含一个线程。进程开启后，会自动创建一个线程，叫做主线程。主线程会启动其他的线程一起执行代码，分配任务。


### 渲染主线程

每一个页面就是会开启一个渲染进程，渲染进程会开启一个渲染主线程，负责页面的HTML，css,js代码执行

事件循环，就是消息循环，渲染主线程会进入一个死循环，不断检查消息队列，有任务就顺序取出任务进行执行，其他线程可以随时向消息队列中，添加任务，如果主线程处于休眠状态，会激活执行任务。

![](.\images\js\Snipaste_2024-03-12_19-11-01.png)

### js执行机制
![](.\images\js\Snipaste_2024-03-18_13-29-42.png)

### js为什么是异步的？

js是单线程语言，运行在渲染主线程中，同步会造成线程阻塞，无法执行其他任务，浪费时间，异步的方式来避免阻塞，提高执行效率。

当开始计时器或点击时，主线程会交给其他线程执行，主线程会执行后面的任务，等其他线程执行完毕，将回调函数包装为任务，放入消息队列结尾，等待主函数执行。

先执行其他任务，最后执行绘制任务，显示到页面上面

任务没有优先级，但是任务队列有优先级，同一类型的任务必须在同一队列，不同类型任务可以在不同队列，也可以不在。

### 任务优先级
主线程中的任务         第一高
微队列中的任务优先级          最高
延时队列         优先级：高
交互队列          中
![](.\images\js\Snipaste_2024-03-12_19-13-22.png)

### js时间不精准

1. 电脑硬件没有原子钟，无法精确计时
2. js调用操作系统的计时函数，会有延迟
3. 计时器循环嵌套，会有4秒延迟
4. 消息队列任务调度过程会产生延迟

# 浏览器渲染原理

### 浏览器如何渲染页面

当浏览器的网咯线程收到HTML文档以后，会产生一个渲染任务，并将其传递给渲染主线程的消息队列。在事件循环机制的作用下，渲染主线程取出渲染任务，开始渲染流程。

整个渲染流程分为多个阶段，分别是：HTML解析、样式计算、布局、绘制、分块、光栅化、画每一个阶段的输入输出，上一个阶段的输出会成为下一个阶段的输入，最后产生像素信息。此为完整流程

#### 1. 解析HTML

解析过程中，遇到css解析css，遇到js解析js。为了提高解析效率，浏览器在开始解析之前，会开启一个预解析线程，率先下载HTML中头部的css文件和外部ts文件。

如果主线程解析到link的位置，此时外部的css文件还没有下载完成，主线程不会等待，继续解析后续的HTML。这是因为下载和解析css的工作是在预解析线程中进行的。这是css不会阻塞HTML解析 原因。

<img src=".\images\img\Snipaste_2024-03-27_22-01-53.png"  />
如果主线程解析到script的位置，会停止解析HTML，转而等待js文件下载完成，并将全局代码解析完成后，才再继续解析HTML。这是因为js代码的执行可能会修改当前的dom树，所以dom树的生成必须暂停。这是js会阻塞HTML解析的原因。   

![]()

第一步完成后，会得到dom树和cssom树，浏览器默认样式，内部样式，外部样式，行内样式都包含再cssom中。
![](.\images\js\QQ截图20240309221517.png)

#### 2. 样式计算style

  将dom树和cssom树整合，得到计算后的样式dom，该过程中，相对单位会变成绝对单位
![](.\images\js\QQ截图20240309221639.png)

******

```
代码
```

#### 3. 布局  layout

布局完成以后会得到布局树。
布局阶段会遍历DOM树的每一个节点，计算每一个节点的集合信息，如每个节点的宽高，相对与包含块的位置等。

大部分的时候，DOM树与布局树不是一一对应的。
比如display:none的节点没有几何信息，因此得不到布局树
比如使用了伪元素选择器，虽然DOM中不存在为原始节点，但是由于含有几何信息，所以会生成到布局树上面。
  ![](.\images\js\QQ截图20240309205735.png)


#### 4. 分层layer

主线程会使用一套复杂的策略对整个布局树中进行分层。
分层的好处在于将来某一层发生变化的时候，仅该层处理，从而提高效率。
滚动条，z-index，transform，opacity等样式会影响分层结果，使用will-change属性可以创建分层
![](.\images\js\QQ截图20240309221804.png)

#### 5. 绘制paint

  主线程会为每一个层单独创建绘制指令集，用于描述如何绘制。
  完成绘制以后，主线程将每个图层的绘制信息提交给合成线程，之后的工作交给合成线程。
  ![](.\images\js\QQ截图20240309211658.png)

#### 6. 分块trling

  合成线程首先从线程池拿出若干线程，对每个图层进行分块工作。
  ![](.\images\js\QQ截图20240309221901.png)

#### 7. 光栅化

  分块完成后，进入光栅化阶段。
  合成线程将信息交给GPU进行，进行高速光栅化，优先处理靠近视口区域的块，最终产生一块块的位图
 ![](.\images\js\QQ截图20240309222109.png)

#### 8. 画 

  合成线程拿到每个块的位图以后，生成一个个指引信息（quad）
  指引会标识每个位图应该在屏幕的哪一个位置，以及考虑旋转，缩放。
  合成线程会将quad信息给GPU进程，由GPU进行系统调用，提交给GPU硬件，完成最终的绘制。
  ![](.\images\js\QQ截图20240309214706.png)
  ![](.\images\js\QQ截图20240309215214.png)

### 什么是回流reflow

回流的本质就是重新计算layout树。
当进行了会影响布局树的操作后，会重新计算布局树，会引发layout。
为了避免多次操作导致布局树反复计算，浏览器会有合并操作，js代码全部执行后，统一进行计算。
所以改动属性造成的回流是异步的。

```
	dom.style.height=
	dom.style.width=
	dom.style.color =   设置，不立即更新
	
	dom.clientWidth    读取，立即reflow
	当js获取属性时，就可能会造成无法获取到最新的布局信息，此时浏览器会立即reflow
```

### 什么是重绘repaint

本质是重新根据分层信息计算绘制指令
当改动了可见样式后，就需要重新计算，就会引发repaint
由于布局信息也属于可见样式，所以reflow一定会引发repaint

### transform效率高的原因

只影响draw画这一步，不影响布局，只是在绘制的时候，改变绘制的位置