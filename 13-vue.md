# vue2.0基础
## 简介
vue是一个js的渐进式框架

## 脚手架

@vue/cli 包

yarn global add @vue/cli

开箱即用，0配置webpack，支持
bable
css,less
开发服务器 等功能

vue create vuecil-demo
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-26_14-43-55.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-26_14-51-29.png)


## MVVM设计模式
数据双向绑定，数据驱动dom，dom赋值数据 

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-28_09-33-50.png)
## vue基础指令

模板字符串语法
### 插值表达式
```
<h1>{{ msg }}</h1>

<script>
export default {
  name: 'HelloWorld',
  data(){
    return{
      msg:'hagallhgal',
    }    
  }
}
</script>

```
### v-bind
标签属性设置变量值
```
<a  v-bind:href='url'></a>   完整写法，冒号属性，传入变量名
<img :src='imgurl'>      简写形式


<script>
    export default {
      name: 'HelloWorld',
      data(){
        return{
          url:'hagallhgal',
        }    
      }
    }
</script>

```
不加冒号，表示直接传入字符串，而不是变量        src='imgurl'
### v-on
给标签绑定的事件

```
export default {
	data(){
		return {
			count :1
		}
	},
	methods:{
		addFun(){
			this.count ++   //data函数会将对象直接挂载到当前组件上面
		}
	}
	
}
```
![](.\images\img\Snipaste_2024-04-28_16-30-58.png)

简写：

@事件名=‘同上’
@click='addFunc'

### 事件对象
事件无传值，可以直接拿，否则需要参数内加参数$event
![](.\images\img\Snipaste_2024-04-28_16-48-53.png)

####  事件修饰符

@click.stop='a'
![](.\images\img\Snipaste_2024-04-28_17-00-18.png)

#### 键盘事件修饰符
![](.\images\img\Snipaste_2024-04-28_17-09-41.png)

#### 原生事件修饰符

原生事件：普通的html标签元素上，浏览器提供的 DOM 事件（click），自动将普通标签上的事件识别为原生事件。

自定义组件的`事件系统(click)`与`原生 DOM 事件（click）`略有不同，同名不同义，不能被浏览器自动识别处理，会被识别为自定义事件，需要组件内部进行处理执行（`this.$emit('click')`）。

`.native` 修饰符更多用于与 Vue 的自定义组件一起使用，特别是当你想要确保事件监听器不会被组件内部的事件处理逻辑所干扰时。或将组件`自定义事件改为原生事件`，此时事件被绑定道元素根元素上。

比如你想要监听一个由第三方库提供的组件的点击事件，但你不想干扰组件内部的事件处理逻辑。

```
@click.native    //组件直接监听一个原生事件

当组件已经开放了click事件时，不需要写.native,直接可以调用组件自定义的click事件

```
### v-modal
接收表单标签 数据，双向数据绑定

默认情况下相当于，向组件传入了value的自定义属性，和名为input的自定义事件

```
<input type='text' v-modal='username'>

<script>
export default{
	data(){
		return username:''
	}
}
</script>
```
#### 修饰符号
接收转换成指定数据格式
![](.\images\img\Snipaste_2024-04-28_17-57-14.png)

### v-text、v-html
显示 指定内容到当前标签的中

### v-show、v-if
显示与隐藏标签

\<a v-show='tag' ></a>  tag为true时显示标签，相当于display：none
v-if 作用一样，相当于直接从dom 树移除

\<a v-if='tag > 2' ></a>
\<a v-else ></a>

### v-for
循环渲染所在的标签

```
<li v-for='(item,index) in arr':key='item'>
	{{index}} - {{item}}   //产生多个li标签
</li>
```

```
<li v-for='index in 10':key='index'>
	{{item}}   //产生多个li标签    输出：1，2，3，4，5，6，7，8，9，10
</li>
```

#### 更新检测
数组变更方法，会导致v-for更新，页面更新
数组非更新方法，返回新数组，不会导致v-for更新。可以采用覆盖原数组或者this.$set(this.arr,0,1000) (位置，值)

### 动态样式
```

<p :class="{className:bool}">      //bool为true，生效样式classname

<p :style="{color:colorParam}">      //colorParam为变量
```

### 过滤器
只能用在插值表达式和v-bind动态属性中

{{ msg | filter（s）}}   //filter为过滤函数
:title="msg | filter"

定义过滤器：
```
Vue.filter("reverse",val =>val.name)   //第一种，定义在main.js文件下，全局定义

filters:{            //第二种、定义在某一文件的配置项中，局部定义
	reverse(val){
		return
	}
}


//传参，s为参数
Vue.filter("reverse",（val，s） =>val.name)   //第一种，定义在main.js文件下，全局定义

filters:{            //第二种、定义在某一文件的配置项中，局部定义
	reverse(val,s){
		return
	}
}
```
### 计算属性
一个变量的值，依赖另一些数据计算的结果
与直接使用函数相比，带缓存只执行一次，简化执行次数

配置项中定义  export default{}
computed:{
	add(){
		return this.a+this.b
	}
}

<a>{{ add }}<a>   //用法类似data中的属性，自动按需调用set,get方法

完整写法：
```
computed:{
	add:{
		set(val){}
		get(){return this.a+this.b}	

	}

}
```
### 监听器
监听data或computed属性的值的该改变

配置项写法：

- 监听简单属性：
  ![watch](.\images\img\Snipaste_2024-04-29_14-07-41.png)

- 监听复杂属性：
  ![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-29_14-12-11.png)
  
  内部简写法：
  
  this.$watch('value',()=>{}),监听的属性，属性变化是执行的函数


### 自定义指令
获取标签，扩展功能
-  全局注册

Vue.directive("指令名"，{
	inserted（el，binding）{   //el是挂载的标签，inserted当被绑定元素插入到父元素中时,进行调用,只触发一次
		el.style.color = 'red'
	},
	update（el,binding，vnode）{   //值或模板更新时，触发此函数，binding为接收参数对象，包含value，name等

​	}
})

- 局部注册
export default {
	directives:{
		focus:{}
	}
}

\<span v-focus = '会发发发'> </span>

## 组件Component
### 使用流程
1、创建组件。包含标签，样式，js代码的一个独立.vue文件
2、注册组件。
全局注册：一般在main.js
	
		```
		 import a from './aa.vue'
	     Vue.component('myComponent',a)   //自定义名字，对象
		```

局部注册:某一文件内
	
		```
		import a from './aa.vue'
	    export default{
	    	components:{
	    		myComponent:a   //名字，对象
	    	}
	    }
		```
3、使用组件。直接当标签使用，<myComponent/>

###渲染到指定位置
渲染到id为app的标签内
```
new Vue({
  el:"#app",
  render: h => h(App),
})   //根实例

```
或
```
new Vue({
  render: h => h(App),
}).$mount('#app')
```
###  关系组件通信
#### 父向子传值

子组件内定义变量
export default{
	props:['title','age']或
	props:{
		title:String
	}
}
父组件传入实参 
<myComponent  title="aga"  age="222"></myComponent>

#### 子向父传值
子组件触发父组件自定义方法

父组件内绑定自定义事件和事件处理函数

```
<myComponent  @subprice='subfn'></myComponent>
export default{
	methods：{
		subfn(a,b){		
		}
	}
}
```
子组件主动触发自定义事件，导致父methods里事件进行执行

```
<button  @click='kanja'></button>

export default{
	methods：{
		kanja(){
			this.$emit('subprice',1,2)
		}
	}
}
```

### 组件间相互更新传值
传统写法：
```
父组件：
<child @myfun='add' :count='mycount'/>
methods:{
	add(value){
		this.mycount = value
	}
}

子:
this.$emit('myfun',false)
```

简化写法：

v-modal或者借助修饰符，.sync

```
父：
<child  :count.sync='mycount'/>
子 
this.$emit('update:count',false)
```

### 无关系组件通信
eventBus，建立的空白对象,组件共用的事件中心。

```
eventBus.js

import Vue from 'vue'
export default new Vue()
```

![](.\images\img\Snipaste_2024-04-29_20-27-33.png)

定义自定义事件---send
![](.\images\img\Snipaste_2024-04-29_20-34-50.png)

### 注册多个自定义组件

other,js
```
import mycomponent from'./'

export default {
	install(vue){
		vue.component(mycomponent1)
		vue.component(mycomponent2)
	}
}
```
main.js
```
Vue.component(other)
new Vue()
```
### 生命周期

![](images/img/Snipaste_2024-08-21_16-13-40.png)

![](.\images\img\Snipaste_2024-04-29_20-58-16.png)

#### created生命周期函数
在组件创建完成后立即执行，可以用于发送ajax请求，用于初始化数据,或注册全局事件

export default{
	async created（）{
		await func(){}
	}
}

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-29_21-14-16.png)

#### mounted()
beforeMount()：真实dom挂载之前进行，用于预处理data，不会触发update钩子函数
mounted()：真实dom挂载之后进行，用于获取dom

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-29_21-25-08.png)

#### update()
beforeupdate（）：数据更新，dom没有更新时才会执行
update（）:dom也更新后才会执行

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-29_21-27-23.png)

#### destroy()

用于手动移除全局事件，定时器，计时器，eventBus移除事件$off方法

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-04-29_21-37-22.png)

#### 激活态与非激活态
配合组件缓存时，查看当前组件切换状态
activated(){}
deactivated(){}
### 接收参数

props：简单数据类型不允许赋值
引用数据类型（数组，对象）可以修改，不能赋值，因为是由父组件传来的值

可用于自定义组件样式

```js
export default {
	props:{
		color:[String,Number]
		bgcolor:{
			type:String;,
			default:'red',
			required:true,
			validator(value){
				return true
			}
		}
	}
}
```
### 动态组件
多个组件互斥显示
```
export default {
	data(){
		return{
			comname:'user'  //变量存当前组件名
		}
	},
	components:{
		user,
		login,
	}
}


//设置挂载点
<component  :id='comname'></component>
```
### 组件缓存
由于上面切换组件，会导致组件频繁的被创建，销毁，效率低下

```
<keep-alive>//缓存对应组件
	<component  :id='comname'></component>
<keep-alive>
```

### 插槽slot
数据不确定时用变量接收，但是组件标签结构不确定时，用插槽
让组件可以接收不同的标签结构显示，对应位置插入对应标签

```
//MyComponent组件
<div>
	<slot>  //占位标签
		<p>默认内容，不传标签时显示</p>
	</slot>  
</div>
```

```
<MyComponent>   //组件内的标签内容，被显示到slot处
	<p>替换可变内容</p>
</MyComponent>
```

#### 作用域插槽
在外部使用插槽，想要访问子组件内部的变量时，可以使用作用域插槽
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-05-06_15-15-35.png)

#### 具名插槽
多个插槽时，可以定义name，进行区分
```
<slot name='body'></slot>

<template  #body/v-slot:body>
</template>

<template  #body='scope'  >
</template>
```
#### 插槽事件

插槽里的事件要定义到当前组件中，而不是子组件中

## 获取原生dom及内容

###获取dom
通过ref或id来获取原生的dom

```
<p  ref="a"   id="b"><p>
this.$refs.a
document.getElementById('#b')
```

获取组件对象中的相关方法:
父组件直接调用子组件的方法，属性

```
<Mycomponent  ref='aaa'> </Mycomponent>

export default{
	methods:{
		subFn(){
			this.refs.aaa.func()
		}
	}
}
```

### 获取dom内容
vue中dom的更新是异步的，数据更新不会立即更新dom，
数据更新后，立即拿dom的内容会导致内容是未更新的

$nextTick作用和生命周期updated作用一致，等dom更新后才执行代码

```
methods:{
	btn(){
		this.$nextTick(()=>{
		
		})
	}
}
```

## 全局变量

简化在每一个文件重复书写，直接定义全局变量，添加到vue原型上
axios.default.baseURL = 'www.sfsffs.com'

Vue.prototype.$axios = axios    //自定义全局变量

```
import a from './aa.vue'
Vue.component('myComponent',a)   //自定义组件名字，对象
```
```
Vue.filter("reverse",val =>val.name)   //第一种，定义在main.js文件下，全局定义过滤器
```

## 路由Router

路径和组件的映射关系

main.js
```
//注册全局组件
Vue.use(VueRouter)
//规则数组
const routes = [
	{
		path:'/find'
		component:a
	}
	{
		path:'/out'
		component:b
		meta:{
			title:'ss'  //当前路由对象的元数据，可以通过route.mate.title访问
		}
	}
]

//生成路由对象
const router = new VueRouter({
	routes:routes
})

//注入到实例中
new Vue({
	router,
	render:h=>h(App)
})

```

app文件
```


//设置挂载点
<div>
	<router-view>
	</router-view>
</div>

```

### 声明式导航

实现自动高亮

可以设置激活样式 .router-link-active{}

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-05-06_21-11-36.png)

###声明式导航 -路由传值
```
<router-link to='/find?name=a'></router-link>

$route.query.name   //获取参数
```
动态路由的方式
```
{
	path:'/out/:id/:name'  //表示要接受含值路径，与/out比是两个路径
	component:b
}


<router-link to='/find/a'></router-link>

$route.params.name   //获取参数
```

### 路由重定向
```
{
	path:'/'，
	name:'index'，
	redirect:'/fing'
}
```
### 404
写在路由规则的最后面，表示全部没有命中
```
{
	path:'*'  
	component:b
}
```

### 路由模式
地址栏的模式

hash路经：10.1.1.1:/#/home,前端访问，路径变化不会经过服务器
history路径：10.1.1.1:/home，后端访问

```
const router = new VueRouter({
	routes,
	mode:'history'
})
```
### 编程式导航
用js代码实现跳转

```
<span @click='btn('/')'>首页<span>

export default {
	methods：{
		btn(rot){
			this.$router.push({path:rot})  或
			this.$router.push({name:'index'})    //建议
		}
	}
}
```
### 编程式导航 -路由传值

```
btn(rot){
	this.$router.push({
		path:rot,
		params:{
			//$route.params.name
		}   或
		query:{
			//$route.query.name
		}
	})
}
```

### 路由嵌套

```
{
	path:'/find'，
	name:'index'，
	component:a,
	children:{
		path:'ranking'，  //不用加 /
		name:'ranking'，
		component:b
	}
}

<router-link to='/find/ranking'></router-link>

```
一级路由:app.vue设置挂载点

二级路由find.vue文件内再次设置挂载二级挂载点

### 激活状态

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-05-06_22-40-23.png)

### 路由守卫
为路由添加权限判断
本质就是一个函数，在路由跳转之前，会执行该函数

- 全局前置守卫, main.js
- 或者在其他文件内写，在main.js导入即可，表示已执行

```
import NProgress from 'nprogress' // progress bar  // 进度条插件
import 'nprogress/nprogress.css' // progress bar style

NProgress.configure({ showSpinner: false }) // NProgress Configuration


router.beforeEach((to,from,next) => {
 	NProgress.start()
	if(to.path === '/my'){
		log(ss)    
		next（）      //正常跳转，不调用next()或传入false，页面停留在原地，阻止跳转，next('地址')，跳转到指定地址
		NProgress.done()
	}
})
```

- 后置守卫

```
router.afterEach(() => {
  // finish progress bar
  NProgress.done()
})

```
## 移动端适配
- 将所有px转为rem
- 利用flexible.js-网页宽度改变，自动切换HTML发font-size

px自动rem
利用webpack配合postcss和postcss-pxtoren
新建postcss.config.js文件，进行配置

```
module.exports = {
    plugins:{
        "postcss-pxtorem":{
            rootvalue:37.5,
            propList:['*']  //所有属性
        }
    }
}
```

## 模块懒加载
```
exportFunc(){
	import('@/a/b.jsx').then(exc =>{
		exc.funcA()   //exc是引入文件的导出对象
	})
}

```
# vuex

## 简介
集中式管理组件依赖的共享数据的工具，可以解决非关系型组件的数据共享问题
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-05-09_10-27-44.png)

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-05-09_10-33-11.png)
## 使用
在main.js中
```
import Vuex from 'vuex'

Vue.use(Vuex)   //全局注册功能 

const store =new Vuex.store({})  //配置对象

new Vue({       //配置到根实例
	store,
	render:h => h(App)
})
```

## state
state存放所有公共状态的属性
```
const store =new Vuex.store({
	state:{
		count:1
	}
})  //配置对象

this.$store.state.count

```

用计算属性，来简化多次重复引用
```
<div>{{count}}</div>
<div>{{count}}</div>

export default{
	computeds:{
		count(){
			return this.$store.state.count
		}
	}
}

更简化写法
import {mapState} from 'vuex'
export default{
	computeds:{
		...mapState(['count'])
	}
}
```
## mutations
存放修改state的同步方法，同步更新，立即执行
```
const store =new Vuex.store({
	mutations:{
		add(state,payload){  //payload为形参
		
		}
	}
})  //配置对象
```

```
export default{
	methods:{
		func(){
			this.$store.commit('add',10)   //调用函数，传入10
		}
	}
}

//简化写法
import {mapMutations} from 'vuex'

@click='func(10,$event)'

export default{
	methods:{
		...mapMutations(['add'])   //调用函数，传入10
		func(a,e){
			this.add(a)
		}
	}
}

```
## actions
存放异步操作，调用mutations的方法
```
const store =new Vuex.store({
	actions:{
		sub(context,payload，){  //context相当于this.store,payload为形参
			context.commit("add",10)
		}
	}
})  //配置对象
```
```
methods:{
		func(){
			this.$store.dispatch('sub',10)   //调用函数，传入10
		}
}

//简写
import {mapActions} from 'vuex'

@click='sub(10,$event)' //不传参，默认第一个参数是$event

methods:{
		
	...mapActions(['sub'])
		
}
```
## getters
作用类似于计算属性，当依赖state中的变量时，可以使用
```
const store =new Vuex.store({
	getters:{
		filterList:state =>state.list.filter(item => item >5)
	}
})  //配置对象

```

```
this.$store.getters.filterList

简写形式 辅助函数
import {mapGetters} from 'vuex'

computed:{
	...mapGetters['filterList']
}
```
## modules
![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-05-09_14-20-33.png)
```

const store =new Vuex.store({
	getters:{
		filterList:state =>state.list.filter(item => item >5)
		name:state =>state.user.name   //建立快捷访问，简化书写
	},
	modules:{
		user:{
			state:{
				name:33
			}
		}
		book:{}
	}
	
})  //配置对象
```

```
<div>{{$store.state.user.name}}</div>
```
## 模块命名空间
子模块的属性定义其实默认均挂载到全局下，可以在全局访问到
mutation，action，getter

this.$store.commit('a')   //a为子模块下的方法

子模块封闭性
```
user:{
	namesapce:true,  //全局空间无法直接调用子模块属性
	state:{
		name:33
	}，
	mutation:{
		a(){}
	}
}
```

actions/mutations可以引入路径

```
test(){
	this.$store.commit('user/a')
}

或者
methods：{
	...mapMutaions['user/a']
}

test(){
	this['user/a']()
}

//创建基于某个命名空间的辅助函数
import {mapGetters,createNamespaceHelpers} from 'vuex'

const {mapMutations} = createNamespaceHelpers('user')  //子模块下的mapMutations，独立于全局
```
getter通过在全局定义快捷访问
```
getters:{
	name:state => state.user.name
}
```

## 持久化
vuex管理状态数据时，同时存储在本地，免去手动
借助插件 vuex-persistedstate（npm i）

```
export default {
	state(){
        return(){
            name:'',
            token:''
        }
	}
}
```

```
import {createStore} from 'vuex'
import {createPersistedstate} from 'Vuex-persistedstate'

export default createStore({
	modules:{
		a,
		b
	},
	plugins:[
		createPersistedstate({
			key:'fsfsfs',  //键名
			paths:['a‘，'b.token']
		})
	]
})
```
默认存储在localstorage

#vue-element-admin

## 简介

vue-element-admin 是一个后台前端解决方案，它基于 vue 和 element-ui实现。它使用了最新的前端技术栈，内置了 i18 国际化解决方案，动态路由，权限验证，提炼了典型的业务模型，提供了丰富的功能组件，它可以帮助你快速搭建企业级中后台产品原型。
不需要什么功能，再删什么

## 哦



- 导入excel

  excel导入功能，需要使用npm包xlsx

  

  文件选择  => FileReader对象得到二进制数据 => XLSX处理二进制数据 => 得到数据

  

  当用户更改 input、select 和textarea元素的值时，`change` 事件在这些元素上触发。

  

  **`FileReader`** 对象允许 Web 应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 [File](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 或 [Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象指定要读取的文件或数据。**`sffs`**

  

  

  ```
   <input ref="files" type="file" v-if="!disabled" class="excelFile" @change="excelFileMethod" />
   
   
   methods:{
   	// change事件
      excelFileMethod(e) {
          var _this = this
          //  excel文件信息
          const files = e.target.files
          console.log(files);
          // 构建fileReader对象
          const fileReader = new FileReader()
          // 读取操作完成时
          fileReader.onload = function(e) {
              try {
                  // 二进制数据
                  console.log(e.target.result)
                   /* parse workbook */
                  const url = "https://xlsx.nodejs.cn/PortfolioSummary.xls";
                  const workbook = XLSX.read(await (await 				fetch(url)).arrayBuffer());
  
                  /* get first worksheet */
                  const worksheet = workbook.Sheets[workbook.SheetNames[0]];
             	  	const raws_data = XLSX.utils.sheet_to_json(worksheet, {header:1});  //从工作表对象生成数据数组
  
              } catch (e) {
                  console.log('文件类型不正确')
                  return
              }
          }
          // 读取指定文件内容，读取完成后触发onload事件
          fileReader.readAsBinaryString(files[0])
          }
  
   }
  ```
  
  

- qrcode
二维码插件
- vue-print-nb
打印插件

nm i vue-print-nb
```
import Print from vue-print-nb

Vue.use(Print) 


<Botton v-print='printObj' > </Botton>

<div id='myPrint'></div>


printObj:{
	id:myPrint
}

```

## 权限控制

RBAC基于角色的权限控制

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-06-03_12-57-52.png)



页面访问权限，按钮操作权限，api访问权限

## mixin混入

让组件可以拥有公共方法,对方法进行复用，相同方法不会覆盖

```
const mixinObj = {
	created(){
		alter(1)
	}
}

const mixinObj1 = {
	created(){
		alter(1)
	}
}

//组件
export default {
	name:'ddd',
	mixins:[mixinObj，mixinObj1]
}

Vue.mixin()
```

## 全屏插件

```
document.documentElement.requestFullscreen()
document.exitFullscreen()
```

npm i screenfull

ScreenFull.toggle()

## 性能分析

npm run preview -- --report

vue-cil可以分析最大的依赖包

### webpack排除打包

大文件放到CDN服务器上，提高访问速度

config.js

```
//定义全局变量
const cdn = {
	css:[]
	js:["http://unpkg.com/element-ui/lin/theme/index.js"]
}


//替换文件
configureWebpack:{
​	externals:{
		'element-ui':'ELEMENT'   //将包名换成cdn的名字
		'vue':'VUE'
​	}
}
```

注入CDN文件到模板

html-webpack-plugin插件

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-06-05_18-58-35.png)

## history页面访问问题

单页面应用，刷新时会查找路径

插件：koa2-connect-history-api-fallback

```
import {historyApiFallback} from 'koa2-connect-history-api-fallback'

const Koa = require('koa')
const serve = require('koa-static')
const {historyApiFallback} = require('koa2-connect-history-api-fallback')
const proxy= require('koa2-proxy-middleware')

const app = new Koa()    //类似于EXpress

app.use(proxy({
	targets:{
		'/prod-api/(.*)':{      //将/prod-api开头的进行代理
			target:'http:www.jj.com/api'
			changeOrigin:true
			pathReWrite:{
				'/prod-api':''
			}
		}
	}
}))

app.use(historyApiFallback()，whiteList['/api']),将所有的网络请求都转发到index.html上，除去白名单上的路由

app.use(serve(__dirname+'/public'))  //导入静态文件

app.listen(3000,()=>{
	log(启动成功)
})
```

##生产环境跨域
借助插件：koa2-proxy-middleware

# vue3.0基础

## vite脚手架

相比于vue-cli更加轻量级，默认安装插件更少

创建项目：npm init vite-app projectName 或者 yarn create vite-app projectName

## 创建应用

```
import {createApp} from 'vue'
import App from 'app.js'

const app = createApp(App)   //创建实例，相当于new Vue()
app.mount('#app')
```

## 组合api

vue2.0是选项api,data,methods,分块

```
export default {
	name:fdfgdf,
	setup（）{
		const a = 1
		return ffff
	}
	
	beforeCreate(){}
}
```

### setup函数
是组合api是起点，执行在beforeCreate之前
函数中的this是undifined
模板中需要执行的函数和变量，在setup中返回
组合api的代码基本全在这里编写

```
<div @click="b">{{msg}}</div>

setup(){
	const a =1
	const b = () =>{}
	console.log(11)
	return {a,b}
}
```

### 生命周期
更新了生命周期函数
setup()
onBeforeMount()
onMount()
onBeforeUpdate()
onUpdate()
onBeforeUnmount()
onUnmount()

可以多次执行同一个钩子，执行顺序未书写顺序

### reactive
复杂对象
创建响应式数据对象，数据驱动视图

```
setup(){
    const obj = reactive({
        a:1
    })
    obj.a = 2

    return {obj}
}
```
从响应式数据对象解构出的属性，不是响应式数据

### toRef
将响应式对象某一属性转换为响应式数据

```
const name = toRef(obj,'a')
name.value = 2

return {name}
```
### toRefs
将响应式数据对象所有属性转换为响应式数据，解析出来

const  obj = toRefs(obj)
obj.name.value = 2
return {...obj}


### ref
普通数据类型
将普通数据类型定义为响应式数据，也可以复杂数据类型

```
setup(){
    const name = ref('ls')
	name.value = 3
    return {name}
}
```
### 父子通信
父向子
父
<Son :mon='mom' @func='func'>

子：
setup(props){
	log(props.mom)
}
子传父
setup(props，{emit}){
	log(props.mom)
	emit('func',50)
}

//双向绑定

2.x中
v-model或者
<Son  :m.sync='n'/>

3.x中
<Son v-model :m='n'/>

### 依赖注入
向后代组件传值
setup(){
	provide('age',age)
}

后代接受值
setup(){
	const name = inject('name')
}

### v-model语法糖更新

input自带input事件，其他组件需要自定义

<Son :modelValue='mom' @update:modelValue='mon=$event'>

<Son v-model='mom'>

父组件不用定义事件，`$event`在自定义方法中表示传参

### mixins语法

公共方法复用

# vue相关原理

![](E:\桌面文件\笔记\前端笔记\images\img\Snipaste_2024-07-24_10-27-54.png)

## 如何实现数据响应式

### 对象属性拦截(Object.defineProperty)--2.x

```
let data1= {}
let a=''
Object.defineProperty(data1,'name',{
	configurable：true,   //可以进行配置
	enumerable:true, ///可以遍历
	get(){
		return a //data1.name自动调用get方法
	},
	set(new){
		//修改属性时自动调用，data.name=1
		此处利用变化，可以进行操作dom ，ajax,
		a = new
	} 
})
```

### 对象整体代理(proxy)--3.X
惰性变化，只有数据被使用到了，才会进行响应式（由于闭包特性，响应式数据常驻内存），缓解无端消耗

```
const data = {}

const proxy = new Proxy(data,{
	get(target,property){},
	set(obj,prop,value){
		if(obj.hasOwnP roperty(prop) && obj[prop] === value){
			//修改属性时自动调用，data.name=1
			发生变化时操作dom,ajax
			document.getElement('p').innerHtml = value
		}
	},
})
```



## 数据变化更新视图
set函数，操作dom    
## 视图变化更新数据
监听事件，input，change



## 发布-订阅模式（自定义事件）
解决数据变化更新视图时，精准更新的问题 ，只更新 属性对应的函数

类似微信公众号，新文章一发布，所有订阅者均会自动获取

document.onClick=（）=>{} 会覆盖，只能执行一次，一对一

document.addEventListener('click',()=>{})，不会覆盖，可以执行多个，一事件对多函数 

定义map，map{'clickevent':[fun1,fun2]}   ,一对多，自行触发 

```
get(){
	//使用了就订阅
	list.add(function())
}
set(){
	//发布所有
	list.forEach(() =>{function})
}
```



# vue中服务端渲染

客户端渲染的vue组件是浏览器通过js渲染的，所以首次加载时间长

解决高效开发，同时加载时间短结合问题，将vue组件渲染的位置放到服务端进行

npm i vue-server-renderer -save

```
服务端

const Vue = require('vue')
const server = require('express')()

const renderer = require('vue-server-renderer').createRenderer()

server.get('*',(req,res)=>{
	const app = new Vue({
		data:{a:2}
		template:"<div>{{a}}</div>"
	}),
	renderer.renderToString(app,(err,html)=>{
		if(err) throw err
		res.send(html)
	})
})

server.listen(888,()=>{  })
```

## 同构

同一份代码（vue,react）,在服务端执行一次，在客户端执行一次

const app = new Vue()

服务端执行为了节约首屏时间

客户端执行为了绑定事件和回复vue本身特性

## Nuxt.js框架

一套使用Vue框架的服务端渲染框架(ssr)

npm create nuxt-app myproject

会进行同构

//异步请求，会在组件渲染前自动进行调用，将安徽之与data(){}进行融合返回给组件
export default {
	async asyncDate({\$axios}){
		const ip = await \$axios.$get('http://')
		return {ip}
	}
	data(){
		return {
			a:2
		}
	}
}
