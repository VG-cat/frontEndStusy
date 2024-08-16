# koa

基于node.js的web框架，有express框架原开发人员打造

```
const koa = require('koa')
const route = require('koa-route')
const static = require('koa-static')
const path = require('path')


const app = new koa()


const main = (ctx) =>{
    if(ctx.request.accepts('html')){    //判断客户端希望接受类型
        ctx.response.type = 'html'
        ctx.response.body = '<h1>fdfdd</h1>'
    }else{
        ctx.response.body = 'fdfdd'  //默认类型text
    }
    
}


app.use(main)

const redirect = (ctx) =>{
    ctx.response.redirect = '/a'  //重定向,状态码302重定向
}


app.use(route.get('/',redirect))  //路由


app.use(static(path.join(__dirname,'/assist')))  //静态资源


app.listen(3000)  //监听端口3000
```



  ## 中间件

**洋葱模型**：

```
const logger = (ctx,next) =>{
	log(1)   //next前的代码，按照app.use的顺序执行
	next()
	log(2)   //next后的代码，按照app.use的相反顺序执行
}

app.use(logger)
```

**异步中间件**：

一个中间件异步，需要把所有中间件改为异步

```
const logger = async function(ctx,next){
	log(1)   //next前的代码，按照.use的顺序执行
	await next()
	log(2)   //next后的代码，按照.use的相反顺序执行
}  
```

**中间件合成**

借助koa-compose包

 compose函数，接受参数为函数，从右向左执行各个函数，初始函数可以接受多个参数，之后的函数只有一个参数

```
const fn = compose（two,one）

const one = (a,b) =>{return a+b}   //初始化函数，并返回值
const two = (c) =>{ return c*10}  //并返回值

fn(1,2)   
```

## 抛出错误

```
const main = (ctx) =>{
	ctx.throw(500)
}

const main = (ctx) =>{
	ctx.response.status(404)
	ctx.response.body('未找到')
}
```

错误处理中间件：

```
const errHandle = (ctx,next) =>{
	try{
		next()
	}catch(err){
		ctx.response.status = err.statusCode || err.status || 500;
		ctx.response.body = {
			message: err.message
		}
	}
}

app.use(errHandle)  //写在最前面
```

错误事件：

```
被catch后，不会触发该事件

app.on('error',err => {
	log(err)
})
```

## 文件上传

koa-body包处理post请求

```
const koa = require('koa')
const route = require('koa-route')
const { koaBody }  = require('koa-body')
const fs = require('fs')
const path = require('path')
const os = require('os')


const app = new koa()


const upload = (ctx, naxt) => {
    const tempdir = os.tmpdir()   //创建临时目录

    const filtsPath = []
    const files = ctx.request.files || {}

    for (const key in files) {
        const file = files[key]

        const reader = fs.createReadStream(file.filepath)
        const writer = fs.createWriteStream(path.join(tempdir, file.originalFilename))
        reader.pipe(writer)

        filtsPath.push(path.join(tempdir, file.originalFilename))
    }

    ctx.body = filtsPath
}


const main = (ctx, next) => {
    ctx.response.type ='html';
    ctx.response.body = '<form id="uploadForm" action="/upload" method="post" enctype="multipart/form-data">\
        <input type = "file" name = "myFile" />\
            <button type="submit">Upload</button>\
            </form > '
}


app.use(koaBody({ multipart: true }))
app.use(route.get('/',main))
app.use(route.post('/upload',upload))

app.listen(3000) 
```

# egg

[官网](https://www.eggjs.org/zh-CN/basics/structure)



阿里基于koa的web框架

写插件，根据业务封装自己的框架，约定优于配置

脚手架

```
npm init egg --type=simple
npm i
```

## 静态文件

![](images/img/Snipaste_2024-08-16_13-14-37.png)



## 控制层

```
const { Controller } = require('egg');

class HomeController extends Controller {
  async templist() {
    const { ctx } = this;
    await ctx.render('demo.nj');
    // ctx.helper.getData();
  }
}

module.exports = HomeController;
```



## 扩展

给ctx扩展方法

```
// extend 类似于util

exports.getData = () => {
  return '工具';
};
```

模板中使用：<h1>你好 {{helper.getData()}}</h1>

控制层使用：ctx.helper.getData();

## 中间件

```
module.exports = (options, app) => {
  return async function(ctx, next) {
    console.log('日志');
    await next();
  };
};

```

```
  config文件
  // add your middleware config here
  config.middleware = [
    'log',
  ];
```



## 模板引擎

npm i egg-view-nunjucks --save

写到app/view 文件夹下

demo.nj

```
<html>
    <head>
        <title>标题</title>
    </head>
    <body>
        <h1>你好 {{helper.getData()}}</h1>
    </body>

</html>
```

## 路由文件

```
/**
 * @param {Egg.Application} app - egg application
 */
module.exports = app => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
  router.get('/list', controller.home.templist);
};
```

## debug

  "debug":"egg-bin debug", 

## 进程通信

相比与node原生的进程通信，增加了agent管理

```
// app.js
module.exports = (app) => {
  // 只有在 egg-ready 事件后才能发送消息
  app.messenger.once('egg-ready', () => {
    app.messenger.sendToAgent('agent-event', { foo: 'bar' });  //只向 agent发送
    app.messenger.sendToApp('app-event', { foo: 'bar' });  //向所有发送
  });
};
```

## 捕获异常

try-catch

**为确保异常可追踪，所有抛出的异常必须是 `Error` 类型，因为只有 `Error` 类型才具备堆栈信息，便于问题定位。**

```js
//跳出异步链的函数异常不会被捕获，需要runInBackground

his.ctx.runInBackground(async () => {        
      // 这里的异常都会被 Background 捕获，并打印错误日志
      await this.ctx.service.trade.check(request);
    });
```

## 404

```
router.get('/404', controller.home.templist);
  
  
  //404自动重定向
 config.notfound = {
    pageUrl: '/404',
  };
```

## egg-mongoose插件

app/modle/ports.js

```
// 该文件表示对应了存储的表为posts

module.exports = app => {
  const mongoose = app.mongoose;

  //定义集合结构
  const postSchema = new mongoose.Schema({
    title: {
      type: String,
      required: true,
      unique: true, // email 必须是唯一的
    },
    content: { type: String },
  });

  //ctx 上可以直接调用 model类 Posts
  return mongoose.model('Posts', postSchema);
};

```
//routes.js

```

/**
 * @param {Egg.Application} app - egg application
 */
 
module.exports = app => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
 
  router.get('/404', controller.home.templist);

  router.resources('posts', '/api/posts', controller.posts);   //定义restful风格接口
};

```
//app/control/ports.js
```

const { Controller } = require('egg');

class HomeController extends Controller {

	async create() {
        try {
            const { ctx, app } = this;
            console.log(ctx.request.body);
            // ctx.validate({ title: 'string' }, ctx.request.body);
            // const postsInstance = ctx.model.Posts({  //自动改为驼峰制
            //     title: ctx.request.body.title,
            //     content: ctx.request.body.content,
            // });
            // const res = await postsInstance.save();

            const res = await ctx.service.posts.create(ctx.request.body);  //service抽离
            this.success(res);
        } catch (error) {
            this.fail(error);
        }
    }
    
	//查全部
    async index() {

        try {
            const { ctx } = this;
            console.log(ctx.body);
            const res = await ctx.model.Posts.find({});
            this.success(res);
        } catch (error) {
            this.fail(error);
        }


    }
	//查单个
    async show() {
        try {
            const { ctx } = this;
            console.log(ctx.params.id);

            const res = await ctx.model.Posts.find({ _id: ctx.params.id });
            this.success(res);
        } catch (error) {
            this.fail(error);
        }
    }
    
    async update() {
        try {
            const { ctx } = this;
            console.log(ctx.params.id);
            // localhost:7001/api/posts/123455
            const res = await ctx.model.Posts.update({ _id: ctx.params.id }, {
                $set: {
                    ...ctx.request.body,
                }
            });
            this.success(res);
        } catch (error) {
            this.fail(error);
        }

    }
    
    async destroy() {
        try {
            const { ctx } = this;
            console.log(ctx.params.id);
            // localhost:7001/api/posts/123455
            const res = await ctx.model.Posts.remove({ _id: ctx.params.id });
            this.success(res);
        } catch (error) {
            this.fail(error);
        }
    }


    async success(data) {
        this.ctx.body = {
            msg: data && data.msg || 'ok',
            code: 0,
            data,
        };
    }

    async fail(data) {
        this.ctx.body = {
            msg: data && data.msg || 'fail',
            code: data && data.code || 0,
            data,
        };
    }

}

module.exports = HomeController;


```

利用service抽离业务，使得control只负责处理http响应和请求

//app/service/ports.js

```


const Service = require('egg').Service;

class PostsService extends Service {

    async create(data) {

        const { ctx, app } = this;

        ctx.validate({ title: 'string' }, data);
        const postsInstance = new ctx.model.Posts({  // 自动改为驼峰制
            title: data.title,
            content: data.content,
        });


        const res = await postsInstance.save();

        return res;

    }

}


module.exports = PostsService;


```

# 接口开发方式



