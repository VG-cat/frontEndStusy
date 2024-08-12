## 简介
第三方包，用于分析和打包代码



## 使用

yarn init

yarn add webpack webpack-cli -D   //-D 开发环境

"script":{   //重命名 命令
	"build":'webpack --config a.config.js'
}

npm run build



npx 可以使用项目中，未全局安装的包的命令（webpack-cli包含命令）

npx webpack

## 配置

修改webpack的出入口文件时，需要新建webpack.config.js文件，进行配置

默认common.js规范

```
默认common.js规范

const path = require('path')

module.export={
	entry:"./src/index.js"      //相对路径
	output:{
		path:path.resolve("./dist/"),  //必须绝对路径
		filename:"bound.js"
	}
	watch:true, //监视文件变化，有变化自动打包
}
```





## 处理css,less文件

style-loader

css-loader

less-loader   //从右往左写，依次链式调用，解析

```
module:{
​	rules:[
		{
			test:/\.css$/,
			use:['style-loader','css-loader'，'less-loader']
		}
​	]
}
```
## 处理字体，图片文件

file-loader

```
module:{
	rules:[
		{
			test:/.(jpg|png|gif|ttf)$/,
			use:['file-loader']
		}
	]
}
```

url-loader

## 处理js，ts文件

bable-loader，将新版语法，转换为旧版语法

npm i bable-loader @bable/core @bable/preset-env -D

```
module:{
	rules:[
		{
			test:/.js$/,
			use:{
				loader:'bable-loader',
				options:{
					presets:['@bable/env']
				}
			}
		}
	]
}
```

某些Es6等新方法，无法进行转换

npm i @bable/polyfill -S

在使用新方法的文件中引入

import ‘@bable/polyfill’

## 开发服务器

实时查看修改，功能类似于watch，自动开启一个前端服务器
webpack-dev-server,内部使用了webpack-dev-middleware

webpack-dev-middleware是一个容器，将webpack处理后的文件发送给服务器

Html-Webpack-Plugin，如果使用了middleware就必须安装，不然HTML文件无法正确解析

```
"script":{
	"dev":"webpack-dev-server --hot --port 500 --open --conentBase src"
}
```

```
const HtmlWebPackage = require('html-web-package')

plugins:[
	new HtmlWebPackage({
		filename:'index.html',
		template:"./src/index.html"
	})
] 
```

单页面应用下，所有的js文件都会被引入到该html文件下

## 打包多页面应用

```
module.export = {
	entry:{
		index：'index.js',
		other："other.js"
	},
	output:{
		path:path.resolve("./dist/"),  //必须绝对路径
		filename:"[name].js"  //多出口
	},
	plugins:[
		new HtmlWebpackPlugin({
			filename:'index.html',
			template:"./src/index.html",
			chunks:['index']  //只引入index.js 
		}),
		new HtmlWebpackPlugin({
			filename:'other.html',
			template:"./src/other.html",
			chunks:['other']
		})
	]
}
```

## 分环境建立配置文件

建立三个文件，base-config.js，dev-config.js，prod-config.js

公共配置：base-config.js，

借助webpack-merge插件

```
const merge = require('webpack-merge')
const baseConfig = require('base-config.js')

module.export = merge(baseConfig,{
	mode:'development',
	devServer:{
		open:true,
		port:3000,
	} 
})
```



```
script:{
	build："webpack --config  prod-config.js",
	dev："webpack --config  dev-config.js"
} 
```



## 跨域问题 

jsonp,已淘汰

cors，服务端处理白名单

http proxy，如下：

开发环境：
webpack的反向代理

```
{
devServe:{
	open:true,
	port:6000,
 	headers:{    //做服务端时，返回响应时，添加白名单（cors）
      'Access-Control-Allow-Origin':["http://localhost:5173"]
    },
	proxy:{     //做客户端需要跨域发请求时,请求转发
		'/api':{   //请求地址中有api,就会触发代理机制
			target:'www.baodu.com'   //域名替换为目标地址
			changeOrigin:true,
			pathRewrite:{
				'^/api':''  //路由替换，localhost:66/api/a =>www.baidu.com/a
			}
		}
	}
}
}
```
生产环境：
nginx的反向代理
此时项目以打包，只有css,html，js

```
server{
	listen:66;
	server_name localhost;
	location ^~/api{
		proxy_pass http://baidu.com
	}
}
```

## production模式打包优化

自带的优化：

- 移除未引用的代码，require()方式导入时不会触发
- 将打散的模块进行合并，减少变量空间占用，只被使用过一次的模块就会被合并
- 代码压缩

## css优化

- 将css提取为独立文件，实现按需加载和sourceMap

  mini-css-extract-plugin插件

```
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
module.export = {
	entry:'./src/index.js'
	plugins:[
		new MiniCssExtractPlugin({
			filename:'[name].css'
		})
	],
	relus:[{
		test:/\.css$/,
		use:[MiniCssExtractPlugin.loader,css-loader,postcss-loader]
	}]
	
}
```

- css自动添加前缀，提高兼容性

  postcss-loader

- css压缩

  bable-loader

## js 优化

代码分离，将js代码分为多块，按需加载或并行加载

常见的代码分离三种方法：

- 入口起点

  entry:{

  ​	index：'index.js',

  ​	map:"map.js"

  }

- 防止重复

  抽取多个js文件中的公共代码,借助插件`splitChunksPlugin`

  ```
  
  optimization:{
  	splitChunks:{
  		chunks:'all'
  	}
  }
  ```

  其他优化项：

  ![](E:\桌面文件\笔记\前端笔记\images\img\QQ截图20240309205735.png)

  

- 动态导入

  import + 懒加载

  npm i -d @bable/plugin-syntax-dynamic-import



```
function getJquery(){
    import('jquery').then((default:$)=>{
    	$('div').addEventListen( )
        return 2222
    })   //返回promise对象
}  


getJquery().then()
```

## 性能优化-noparse

对于一些独立的第三方库，库本身没有在依赖其他库，如jQuery，我们就没必要再解析库内部的依赖关系

```
module:{
	noParse:/jquery|bootstrap/
}
```

## 性能优化-lgnorePlugin

对于一些第三方包，我们不需要其所有依赖，可以全部忽略，再按需导入

```
import 'moment/loacal/zh-CN'


webpack.js-
plugins:[
	new webpack.IgnorePlugin(/\.\/local/,/moment/)
]
```

 

## 性能优化-DllPlugin+DllReferencePlugin

对于某些第三方库，我们只需要打包一次，之后不需要再每次打包构建，如react，vue等。

此时可将这些库作为动态链接库ddl，

```
vue-config.js:只需要执行一次

const path = require('path')
const webpack = require('webpack')

module.exports = {
	mode:'development',
	entry:{
		vue:[
			'vue/dist/vue.js',
			'vue-router'
		]
	},
	output:{
		filename:'[name]_dll.js',
		path:path.resolve(__dirname,'../dist'),
		libiary:'[name]_dll'   //在全局暴露出该文件，在html页面中的引用才会识别
	},
	plugins:[
		new webpack.DllPlogin({
			name:'[name]_dll',   //vue_dll
			path:path.resolve(__dirname,'../dist/manifest.json')    //清单文件，该文件记录了打包后，模块之间的对应关系 
		})
	]
}
```



```
base.config.js,引入动态链接库

plugins[
	new webpack.DllReferencePlugin({
		manifest:path.resolve(__dirname,'../dist/manifest.json') //将vue_dll引入到打包文件中
	})
]
```

```
npm i add-asset-html-webpack-plugin -D

base.config.js
将DLL引入通过script标签自动添加大html文件中

plugins :[
	new AddAssetHtmlWebpack({
		filepath:path.resolve(__dirname,'../dist/vue_dll.js')
	})
]
```

## 浏览器缓存问题

在不重启服务器，直接修改服务器中的代码时，浏览器会优先访问缓存中的js,css，图片等文件，而不是服务器中的最新代码。

同时浏览器是利用名字进行查找的，如果在缓存中可以找到该文件名字且缓存没过期，就从缓存中拿取。

所以在每次打包时，对可能变化的文件，作动态名字处理。

```
module.exports = {
	output:{
		path:path.resolve("./dist/"),  //必须绝对路径
		filename:"[name].[contenthash:8].bundle.js"  //hash名字
	}
}
```

