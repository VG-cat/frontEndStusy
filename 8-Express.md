##简介

express是基于node.js的web开发框架
作用与node.js内置的http模块类似，是专门用来创建web服务器或API服务器

## 安装

npm i express

##创建基本web服务器

const express = require('express')

const app = express()

app.listen(80,()=>{  //启动成功的回调函数
	
})

启动后，以启动项目文件所在位置为文件起点，./为当前目录


## 托管静态资源

express.static()可以方便的创建静态资源服务器，以对外开发

app.use(express.static('public'))    //将public文件夹下的资源公开，public不会出现在访问路径里

如访问：localhost:80/image/a.css

app.use(express.static('flies'))    //两个文件夹都托管
app.use(express.static('public'))

app.use(/public,express.static('public'))  //添加统一路径前缀

路径不能通过相对地址得到的父级目录，只能通过相对路径调用当前目录
想要得到父级目录，必须通过绝对路径，__dirname+../public

## 路由

### 挂载路由

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-15_20-04-58.png)

路由函数/接口
app.get('/',function(req,res){

})


### 路由匹配

按照路由书写先后顺序，进行匹配
所以短路由写在前面

路由支持字符串模式匹配和正则化
/a?b     a可有可无
/a+b   //任意多个a,至少一个
/a\*b       ab,中间任意多个字符
/a(bc)?d      bc为一组，可有可无



### 模块化路由中间件

不建议直接将路由挂载到app上面，而是推荐将路由抽离为单独的模块

1、创建js文件，
2、调用express.Router（）
3、module.exports()，向外共享

- 路由文件.js

```js
const express = require('express')

const router = express.Router()

router.post('./:id',(req,res)=>{
	const obj  = req.query  //获取路由中的查询参数
	
	const obj  = req.params()  //匹配动态参数，id
    
    const obj  = req.body()  //请求体中的数据
	
	res.send('hello')
})



module.exports = router
```

- 启动文件
```js
const userRouter = require('./router.js')

app.use(‘/api’,userRouter)   //注册自定义的路由模块,所以路由添加统一前缀，/api

```

## nodemon

监听项目文件变动，有变动则自动重启文件，不用手动重启

npm安装

执行：
nodemon study16-Express\app.js

## 中间件（middleware）

指业务流程的中间处理环节

![](.\images\img\Snipaste_2024-04-16_10-59-33.png)

![](.\images\img\Snipaste_2024-04-16_09-41-00.png)

<b>next函数</b> 的作用就是实现多个中间件的串联调用，表示把流转关系转交给下一个中间件或路由,没传入中间件时匹配路由

### 基本语法

const mw = function(,req,res,next){
	log(aaa)
	next()    //表示继续转交，一定要有,里面有参数时，给下一个中间件的第一个参数err
}

### 全局生效的中间件

全局生效的中间件  ,  即客户端的任何请求到达服务器后,都会触发的中间件
app.use(中间件函数)    //挂载全局生效的中间件

app.use(mw1)    //多个中间件顺序执行
app.use(mw2)  

### 局部生效的中间件

只在当前路由中生效的中间件
app.get('./use',mw1,mw2，function(req,res){})

### 注意点

1、一定路由之前注册中间件，除了错误中间件
2、中间件里一定要有next(),且写在最后面，
3、中间件里不要有res.send()，否则直接发送到页面，中断后续执行
4、多个中间件通信，共享req，res对象，上游挂载额外属性，下游使用

### 中间件分类

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_13-21-12.png)

应用中间件：绑定到app实例上面的中间件
路由中间件：绑定到Router()实例上的中间件
错误中间件：用来捕获异常错误，function（err,req,res，next）{}，必须注册在所有路由之后
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_13-35-31.png)

内置中间件：
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_13-54-37.png)

### 自定义中间件

```js
app.use((req,res,next)=>{
	let str = '',  //文件大的时候会切割文件，有多次data事件
	req.on('data',(data)=>{
		str+= data
	})
	
	req.on('end',()=>{
		数据全部发送完毕，触发
        req.body = str   //挂载为req的属性，供下游使用
	})

})
```

也可以单独封装一个文件，利用module.exports = {}  导出

## Express接口

和路由函数一样
- 服务端

Router.get('/api',function(req,res){
	res.send('aaa')
})
- 客户端

$.ajax({
	type:'GET',
	url:'http://127.0.0.1:8080/api'
	data:{}
	success:function(res){
		log(res)
	}
})

但是上面写法会出现不支持跨域问题

### CORS

只在服务端进行配置，且要求浏览器支持xmlHttpRequestLevel2

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_19-08-54.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_16-52-08.png)

#### 响应头-个性化

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_19-11-41.png)

![Snipaste_2024-04-16_19-14-23](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_19-14-23.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-16_19-17-09.png)

### JSONP

jsonp只支持get方式

浏览器通过\<script>标签的src属性，请求服务器上的数据，同时服务器返回一个函数调用

如果已经配置了cors，为了防止冲突，必须在配置Cors中间件之前声明JSONP的接口，否则会被处理为开启了cors的接口

```
app.get('./jsonp',function(req,res){
	const fname = req.query.callback    //获取名字
	const data = {}    //返回的数据
	
	const str = `${fname}(${json.stringify(data)})`   //拼接函数调用字符串
	
	res.send(str)     //返回
})

$.ajax({
	method:'GET'
	dataType':jsonp'
	url:'127.0.0.1:8080'
	success:(res)=>{}
})

```


## 服务器端渲染模板

npm i ejs

### 配置引擎
app.set('view','./view')    //模板文件目录
app.set('view engine','ejs')  //模板引擎

### 使用
app.get('./',(req,res)=>{
	res.render('index')   //自动去找view页面下的index.ejs文件
})

### 语法
.ejs里面和html一样，只是有额外语法

res.render('index',{title='222'})     //传入参数

\<h1><%= title%></h1>  //页面接收参数

<% %> //流程控制标签，写if-else，for等，包头包尾，包中间数据，不包html标签
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-17_13-39-52.png)
<%= %> // 原文输出
<%-  %> //解析为Html
<%# %>  //注释，开发者可以看到，页面看不到，压缩文件大小
<%-include('./header.ejs'，{params:eee}) %>   //导入公共文件header.ejs文件

res.locals.message = ''  //将数据存在locals中，在使用模板时，不需要传入参数{params:eee}，可以直接使用

### 直接渲染html文件

app.set('view','./view')    //模板文件目录
app.set('view engine','html')  //模板引擎
app.engine('html',require('ejs').renderFile)  //自定义一个html引擎，支持直接渲染HTML文件

## Express生成器
脚手架，可以快速创建一个应用的骨架

npm i express-generator

express myapp --view=ejs

## mysql模块

npm i mysql

```
const mysql = require('mysql')

const db = mysql.createPool({
    host:'127.0.0.1',
    port:3306,
    user:'root',
    password:'123456',
    database:'bookstore'
})

db.promise() //支持promise
```
cmd,更改验证方式：
alter user 'root'@'localhost' identified with mysql_native_password by '123456';

用？占位

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-19_14-38-16.png)

const  user  = {username:22}
const sqlStr = 'inster into users set ?'
db.query(sqlstr,user,(err,resultt)=>{})   //简化写法

## multer模块
用于在服务端，保存文件到对应文件夹，数据库存相应文件的地址即可


const multer = require('multer');

const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'upload/');
    },
    filename: (req, file, cb) => {
        cb(null, file.fieldname + '-' + Date.now() + path.extname(file.originalname));
    }
});

const upload = multer({ storage: storage });


api.post('/publishArtical', upload.single('avatar'), (req, res) => {
    console.log(req.file);
    let image = req.file;
    const imagePath = image.path; // 从Multer的文件对象中获取路径
    console.log(req.file);
    db.query('insert into artlist set ?', { ...req.body, 'avatar': imagePath }, (err, result) => {
        console.log(err);
        if (err) {
            return res.send({
                status: 500,
                message: '发布失败'
            })
        }
        if (result.affectedRows === 1) {
            // 用户名和密码匹配，生成token
            return res.send({
                status: 200,
                message: '发布成功',
            })
        }
    })


})