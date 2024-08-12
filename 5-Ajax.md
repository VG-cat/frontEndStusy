## url 
![](E:\桌面文件\笔记\前端笔记\images\js\QQ截图20240325093845.png)

域名：标记服务器在互联网中 方位
资源路径：标记资源在服务器下的具体位置

##  HTTP
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_14-45-41.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_15-19-45.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_15-31-01.png)

### 查询参数
？参数1=值1&参数2=值2

### 请求方法

get：获取数据
post：提交数据
put：修改数据(全部)
delete：删除数据
patch：修改数据（部分）

# AJAX
异步的js和xml，利用XMLHttpRequest对象与服务器进行通信，在不刷新页面的情况下，获取数据，进行更新

## jQuery中的Ajax

### $.get()

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-10_19-50-21.png)

### $.post()

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-10_20-01-35.png)

### $.ajax()

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-10_20-05-14.png)

```
$.ajax({
  url: '服务器接收数据的URL', // 服务器端接收数据的URL
  type: 'POST',
  data: serializedData,
  contentType: 'application/x-www-form-urlencoded; charset=UTF-8', // 设置请求头的 Content-Type
  dataType: 'json', // 预期服务器返回的数据类型
  success: function(response) {
    // 请求成功后的回调函数
    console.log('数据提交成功:', response);
  },
  error: function(xhr, status, error) {
    // 请求失败后的回调函数
    console.error('数据提交失败:', error);
  }
});
```



## 模板引擎

根据指定的模板结构和数据，自动生成一个完整的HTML页面


### art-template 模板引擎

、、、
<script src='./template-web.js'></script>   //导入文件
<script type='text/html'  id='nihao'>   //构建模板

	<h1>{{name}}</h1>
</script>


<script>
 	varl  HTMLstr  = template('nihao',{name:'李欢'})    ///生成页面，传入对象参数
 	dom.html(telHTML)   //使用
</script>
、、、

### art-template标准语法：

####原文输出，
不再按字符串处理   
{{@ value}}   '<h1>niha</h1>'

####条件输出：
{{if v1}}   输出内容1 { {else if v1}}  输出内容2   {{/if}}

####循环输出传入：{arr:[]}
{{ each arr }}
	{{\$index}}  {{\$value}}
{{/each}}

#### 过滤器、处理器
{{value  | filterName}}

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_14-47-25.png)

### 模板原理
正则表达式检测，替换为真实数据

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_15-04-06.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_15-08-26.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_15-15-43.png)

## form-serialize

### form-serialize  插件

快速获取表单的值

```
const form = document.queryselector('form')
const data = serialize(form,{hash:true,empty:true})     //返回对象  

```

### jQuery里seialize()

$('.form').serialize()          //返回字符串   username=''&age='123'

 $("#form1").serializeObject()    //返回json对象

 $("#form1").serializeArray() 

form标签属性:
enctype：发送数据前如何对数据进行编码
有三个可选值，默认application/x-www-form-urlencoded，不上传文件时这么用
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-10_22-30-17.png)

 对于使用 `$.ajax` 方法提交表单数据的情况，`enctype` 属性通常不需要设置，因为 `$.ajax` 会使用 `FormData` 对象来收集和发送表单数据，而 `FormData` 对象会自动处理数据的编码方式。如果需要上传文件，你只需要确保在创建 `FormData` 对象时包含了文件输入控件选中的文件即可。 

 对于原生表单提交（例如使用 `<input type="submit">`），如果表单中不包含文件上传控件，通常不需要显式设置 `enctype` 属性，因为默认的 `application/x-www-form-urlencoded` 就足够了。如果表单中包含文件上传控件，那么你需要将 `enctype` 设置为 `multipart/form-data`，以确保文件数据能够正确传输。 

 ### URL编码 `urlencoded` 编码 

URL地址中只允许有英文，数字和符号，浏览器自动进行编码
在url中，用英文字母表示非英文字母，如中文，韩文等

url编码
encodeURI('你好')      //“%E1%B3%32

url解码
decodeURI('%E1%B3%32')   // 你好

## xhr  --XMLHttpRequest

XMLHttpRequest是浏览器提供的js对象，用于向服务器请求数据

### GET请求
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_15-32-50.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_15-35-27.png)

### POST请求

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_15-57-49.png)


## XMLHttpRequest  Level2

新增功能：
1、可以设置HTTP请求时限
2、可以使用FormData对象管理表单数据
3、可以上传文件
4、可以获取数据传输的进度

 

### 请求时限

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_20-14-22.png)

### FormData

H5新增FormData对象

`FormData` 对象可以编码数据，但它默认的编码格式是 `multipart/form-data`，这种格式特别适合于包含文件上传的表单数据。当你向 `FormData` 对象中添加数据时，如果数据是文件，浏览器会自动处理文件的二进制数据和必要的边界字符串。对于非文件类型的数据（如文本输入字段），`FormData` 也会正确地将它们编码为键值对形式，但仍然使用 `multipart/form-data` 格式。

如果你需要将数据编码为 `application/x-www-form-urlencoded` 格式，你不能直接使用 `FormData` 对象，因为它不支持这种格式

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_20-23-09.png)

### 上传文件

document.querSelecter('#files1').files    //获取文件列表

将文件追加到formData对象中，重复上面post操作即可

### 传输进度


![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_20-44-51.png)

传输完成后执行
xhr.upload.onload = function(){}

## jQuery文件上传

如果你使用 `$.ajax` 并且设置了 `contentType: false`，就不会与 HTML 表单的 `enctype` 属性冲突，因为 jQuery 会忽略 `enctype` 并使用 `FormData` 来发送数据。

 `contentType: false` 确保了 jQuery 不会尝试设置 `Content-Type` 头部，而是允许浏览器根据 `FormData` 对象自动生成正确的 `Content-Type` 头部，包括边界字符串， 



![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_21-14-19.png)

加载效果
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_22-42-01.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_22-44-36.png)

## 数据格式转化

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-11_17-59-53.png)

## axios

专注于网路请求的库，
相比于原生的XML对象，简单易用
相比于jQuery，轻量化


axios.get(url,params)
axios.post(url,data)

```
axios({
	url:'目标资源地址'
	method:'请求方法，默认get（不区分大小写）'
	//get请求参数
	params:{
		参数：值
	}
	//post请求参数 
	data:{    //get，delete时不需要写
		参数：值
	}
	
}).then(result =>{

}).catch(error =>{

}) 
```

## 同源 与 跨域

如果两个页面的协议，域名，端口相同，则两个页面同源

浏览器只允许同源网站进行交互

跨域：有其中一项不相同就为跨域

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_09-44-12.png)

跨域解决方案

JSONP：只支持GET
CORS ：支持GET和POST，但不兼容低版本浏览器

### JSONP
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_09-51-00.png)

使用方法

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_10-12-36.png)

jQuery中的JSONP

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-12_12-20-37.png)

