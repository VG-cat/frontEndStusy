## 模块化

![](.\images\img\Snipaste_2024-04-25_14-48-20.png)

官方模块化规范，ES6模块化规范
前后端通用
![](.\images\img\Snipaste_2024-04-25_15-01-12.png)

package.json中的根节点加    "type": "module", 将node.js改为es6模块化

### 默认导出		
一次
export default {

}

### 默认导入

用a1接收名字
import a1 from './fjakfjahf.js'

### 按需导出
多次
export let s1= 'aaa'
export let su1= 'aaa'

### 按需导入
import {s1 as s} from './dhfsihj.js' 
重命名

## promise

Promise 是一种用于异步计算的对象。它代表了一个可能现在不可用，但预期将来会可用的值，或者一个在未来某个时间点才可用的结果。

解决了层层嵌套，回调地狱的问题



promiseObj
.then((success)=>{},(err)=>{})  //等待执行，then的第一个参数为resolve，第二个参数为reject。
.then((success)=>{},(err)=>{})   //第二个参数可以省略，then返回一个promise对象 
.catch(err =>{   //捕获异常，如果then（）没有第二个参数，也会捕获reject中的信息
	log(err.message)
})



### 方法

Promise.all(opt).then()    //发起并行异步操纵，opt为promise对象数组，当opt中所有的异步操纵并行执行完毕后，才会执行then

Promise.race(opt).then()    //发起并行异步操纵，其中任意一个执行完毕后，就会执行then



Promise.resolve()

Promise.reject()

### 封装
promiseObj = new Promise(function(resolve,reject){})

resolve,reject交给.then(（）=>{},()=>{})调用，

reject抛出的error,交给.catch()调用    //resolve和reject两者只会执行一个，不做判断t/f时，谁写在前面，执行谁。

```
const getFile(){
	return new Promise(function(resolve,reject){
		if(success){
			resolve(1)
		}else{
			reject(2222222222)
		}
	})
}

getFile()
.then(res=>{
	log(res)        //1，为resolve传入的参数
	return res+1
})
.then(res=>{
	log(res)        //2
	return res+1
})
.then(res=>{
	return Promise.reject(new Error(‘失败’))   //强制终止执行链
})
.catch(res=>log(res))   //失败，捕获错误

```

```
const util = require('util');

// 定义一个遵循 Node.js 回调风格的函数
function originalFunction(n,callback) {
    // callback('erroe', 'hello'); // 调用回调函数,----------2 error
    callback(null, 'hello'); // 调用回调函数--------2 hello
    console.log(n);    // 打印 'hello'
}

// 将回调函数转换为返回 Promise 的函数
const fun = util.promisify(originalFunction);

// 调用转换后的函数
fun(2).then((result) => {
    console.log(result); // 输出: hello
}).catch((err) => {
    console.error(err);
});
```



## async/await

简化promise只能链式.then的问题，await包裹的是一个promise对象

async/await  让异步代码的写法看起来是同步执行的，但实际上它仍然是异步执行的。 
异常通过try-catch捕获

async function(){
	log(a)
	const r1 = await promiseObj（）   //直接返回内容，不是promise对象
	const r2 = await promiseObj（）
}	

await修饰后，await之前的代码同步执行，之后的代码异步执行
同时函数前用async修饰
![](.\images\img\Snipaste_2024-04-25_17-00-20.png)

并行问题：

```
async getAll(){
	const r1 = getFunc(./a)
	const r2 = getFunc(./b)
	const r3 = getFunc(./c)
	const p = await Promise.all([r1,r2,r3])   //1,2,3不在串行发送
	log('全部完成')
	
}
```



## Eventloop

就是事件循环

## 宏任务和微任务
异步任务分为两大类
![](.\images\img\Snipaste_2024-04-25_18-53-08.png)

有微任务就先执行微任务，微任务全部执行完成后执行宏任务



## 请求节流，一个接一个发

```
function OriginFatch(url){
	log(url)
	return new promise(function(reslove,reject){
		reslove(url)
	})
}


function fatchOne(url){
	if(fatchOne.aa){
		fatchOne.aa = OriginFatch(url)
	}else{
		fatchOne.aa = fatchOne.aa.then(OriginFatch(url))
	}
	
}

fatchOne(1)
fatchOne(2)
fatchOne(3)
```



## 请求节流，两个一起发

```

function fatchTwo(url){
	if(fatchTwo.arr >=2){
		const temp = fatchTwo.arr.splice(0,2)
		temp.map(item=>{
			OriginFatch(item)
		}) 
		if(fatchTwo.bb){
			fatchTwo.bb =Promise.all(temp)
		}else{
			fatchTwo.bb =fatchTwo.bb.then(()=>{
				Promise.all(temp)
			})
		}
		
	}else{
		fatchTwo.arr.push(url)
	}
	
}
```



## axios

axios是一个专门用于发送ajax请求的库,ajax+promise
- 支持客户端和服务端发送请求
- 支持promise语法
- 支持请求和响应拦截
- 自动转换json数据

![](.\images\img\Snipaste_2024-04-29_21-57-25.png)

全局配置：

或者定义到环境变量配置文件中env

axios.defaults.baseURL = '10.2.3.3'

```
const service = axios.create({
  baseURL: process.env.VUE_APP_BASE_API, // url = base url + request url
  // withCredentials: true, // send cookies when cross-domain requests
  timeout: 5000 // request timeout
})
```

·响应拦截器

````
// response interceptor
service.interceptors.response.use(
  response => {
    const res = response.data

    if (res.code !== 20000) {
      return Promise.reject(new Error('Error'))
    } else {
      return res
    }
  },
  error => {
    console.log('err' + error) // for debug
    return Promise.reject(error)    //不执行.then，执行catch
  }
)
````

·请求拦截器--权限拦截样例

```
// request interceptor
service.interceptors.request.use(
  config => {
    // do something before request is sent

    if (store.getters.token) {
      // let each request carry token
      // ['X-Token'] is a custom headers key
      // please modify it according to the actual situation
      config.headers['X-Token'] = getToken()
    }
    return config
  },
  error => {
    // do something with request error
    console.log(error) // for debug
    return Promise.reject(error)
  }
)

```

## fetch

```
fetch(`https://geoapi.qweather.com/v2/city/lookup?location=${location}&key=${KEY}`
,{
        method: 'GET',
}).then(res => res.json())
.then(res => setdata(res));    //需要处理，才可以返回promise类型
```

