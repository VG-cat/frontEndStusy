## react简介：
用于构建用户界面的js库，操作DOM显示界面
所以，发送请求，处理数据需要自行处理（axios等）
react native 可以进行移动端开发，组件化模式，声明式编码

优势：虚拟dom进行比较，如果一致，则不重新创建新的，所以会高效率。

工具：react-tool-dev  facebook出品

## jsx语法：
标签首字母小写时，对应HTML标签 <h1>
标签首字母大写时，对应组件 <H1>

/<h1>{obj}</h1>

列表自动遍历，对象不会自动遍历

## 组件
### 函数式组件:
function Demo(){
	return <h1>组件</h1>
}
一定要有return
<Demo/>

### 类式组件：

里面只能写构造器，自定义方法，变量

class Demo extends React.compontent{
	constructor(props){
		super(props)
		this.states = {isHot:true}
	}
	//自己函数的写法
	happy=（）=>{
	
	}
	//生命周期回调函数
	//组件完成挂载时，进行调用   (只执行一次)    系统方法
	comptentDidMount(){
	
	}
	//组件将要卸载时，进行调用   (只执行一次)    系统方法
	comptentWillUnmount(){
	
	}
	//覆写render方法        系统方法     调用：初始化渲染和状态更新时
	render(){
		return<h1>组件+{this.isHot}<h1>
	}
}

<Demo/>

### 组件三大属性：
#### props：参数，外部
<MyComptence name='Tom' /> 将name作为键值对传入到类内，调用的时候，this.props.name 即可  

- 限制参数类型：
	类的外部、
    Demo.propTypes = {
        name:React.PropTypes.string[/.isRequired],  //String类型，是否必须传入
        speak:React.PropTypes.func
    }
  
    简写 类内部增加
    class Demo {
    	//构造器参数中包含 默认参数
    	constructor(props){
    		super(props)
    	}
        static propTypes = {
        	name:React.PropTypes.string[/.isRequired],  //String类型，是否必须传入
    	}
    	startic defultProps={}
    }
  
- 默认参数
    Demo.defultProps={
        name:"DH",   
    }

//函数也有props，但没有state 和ref，需要借助hooks
function Peron(props){
	const {name,age} = props
}

#### states：内部，状态，驱动页面变化      
 状态不可以直接更改(state.a=b)，需要借助react内置api，this.setState({a:b})

每次改变状态，会重新执行render函数进行渲染 

#### ref
类似与id，通过ref来获取当前的节点,组成节点键值对refs{a:input}
- 1 字符串ref  已经过时
  class Demo {
		show(){
			const {div1} = this.refs
			alart(div1.value);
		}
		
	
  	//refs :{'div1':div}
  	render(){
  		return (
  	
  			<div ref='div1'>你好</div>
  		)
  	}
    }
- 回调ref
	render(){
    		return (
    		//回调函数，currentNode表示当前节点
  
    			<div ref={(currentNode) =>{consloe.log(currentNode)}}>你好</div>
    		)
    	}
- createRef
     const myRef = React.createRef()  //创建一个保存节点的容器
     <div ref={this.myRef}>你好</div>
     

不要过度使用ref
当自身事件改变，操作自身的时候可以省去ref，直接获取                     (他人的事件，操作自己可以用ref)
<input onClick={(event) =>{console.log(event.target.value)}}/>


##  副作用Effect
当一些函数需要调用setState时，可能会出现循环渲染的问题，React-strictModal会突显这种副作用，副作用检查机制，执行两次。
在函数组件的函数体里直接执行setState，会出现重复渲染问题。（在渲染状态时，执行setState）

    useEffect({
     ******
     return （）=>{}
    },[])

useEffect是在组件渲染完成后，才会执行，所以解决了重复渲染的问题useEffect(()=>{},[a,b])，

- 当依赖项a，b发生变化时，就重新会执行函数，

- 不加[]时，默认依赖项为包裹函数中的所有变量，

- 加空[]时，只会在初始化时执行一次

return 是清除函数，先执行清楚函数，然后执行useEffect函数体，用于去除前一次useEffect函数的影响


State 有两种状态，
-  渲染状态：return函数还未执行，组件还未显示到页面中，
	此时调用setState,不会检查state是否重复，setState会重复渲染页面，
- 非渲染状态：组件已经显示到页面中，
	此时调用setState会检查State的值，如果发生变化，会重新渲染，
	如果没有发生变化，不会进行重新渲染(也可能只渲染当前组件，不渲染子组件，不进行刷新)
	
##  整合器Reducer
当我们对state进行多种操作时，只用setState（），会繁琐，数据与操作分散，到处瞎写
一般定义在组件外部，减少创建次数（防止每次调用组件，创建一次）

const [count,countDispatch] =  useReducer( reducer：(state,action)=>{
	if(action.type==='add'){
		return state+1;
	}
	return state
}, initialArg:1)

useReducer是对功能进行整合:
- 接收两个参数，一个为reducer函数，另一个为count的初始值
- 返回两个值，一个为state，另一个为派发器，执行reducer中的函数

reducer函数:
- 接收两个参数，一个是最新的state，另一个为action
	action:{type:'add'}
- 返回值，return 赋值到count

使用：
	countDispatch（{type:'add'}）
	
## React.Momo 包装组件
组件渲染有两种情况，自身组件发生变化，进行渲染，第二种，父组件发生变化，进行渲染
当第二种情况时，子组件没必要渲染，
使用React.momo(Copetement)进行包装，对组件Copetement进行缓存，
只有当组件的props发生变化时，才会重新进行渲染，非必要不会重新渲染
适用于复杂组件，渲染时间较慢


使用：export default React.momo(Copetement)

##  useCallBack
子组件对父组件传值时，利用函数进行转递，
但是当父组件也用这个函数操作时，父组件会因为state更新重新渲染(所有代码全部重新执行)，
回调函数也会重新创建，子组件也会同样进行重新渲染，
回调函数每创建一次，子组件props发生变化，所以每次子组件会重新渲染
此时React.momo无法阻止重新渲染


原来写法：
const handle =( ) =>{
	回调函数
}，
改良写法：
const handle = useCallBack(( ) =>{
	回调函数
},[ ])

useCallBack接收两个参数:
- 回调函数，供自己使用或传递给子组件的函数
- 依赖项：
	- 无依赖项：每次渲染页面都会重新创建该回调函数，相当于原来直接使用的，父组件，子组件一起渲染
	- []，空数组，只创建一次，页面重新渲染不会执行，因此子组件props不变，不会再渲染子组件，只渲染父组件
	- [a,b]，当依赖项发生变化时，就会重新创建回调函数，重新渲染子组件，父组件

##  自定义钩子
类组件中，钩子函数的this指向是类组件的实例对象
钩子函数只能在**函数组件**中或者**自定义钩子函数**中使用
自定义钩子函数就是一个普通函数，只是名字为use开头
直接return对象就可以，提取组件中的重复代码

两次传参数的问题，在哪里调用返回的函数，就把具体参数传入，否则会出现没传入的问题



## Redux 可控状态容器
为解决state在组件之间传递繁琐，管理繁琐，将context和reducer结合，定义了外部状态仓库，统一管理state，可预测就是在外部无法改变state值，只能在A中使用定义好的action操作

使用：
1. 定义整合器
	与reducer基本类似，包含两个参数，state和action
	function A（state:{count:1}，action:{type}）{
		swich(action.type):
   			case 'ADD':	
            	return state 
	}
	
2. 创建store仓库
  接收两个参数，整合器和状态初始值
  const store = Redux.createStore(A，{count:1})

3. 创建订阅函数
  当state的值发生变化时，就会触发执行该函数
  store.subscribe(() =>{
  	console.log（store.getState()）//  拿到整合器中的return值
  })

4. 利用派发器进行调用
  const  B  = () =>{
  	store.dispach({type:'ADD'})
  }
##  RTK（Redux toolkit）
除去重复型操作，简化redux的操作，可以处理多个state，而不是分组处理，
分组处理：定义多个Reducer，多个Reducer组成一个state，重复上述操作

1、创建reducer切片，每一个slice单独一个文件，导出
需要一个配置对象作为参数，通过对象的不同属性来指定配置
const stuSlice = createSlice({
	options:{
		name:'stu',  //唯一，用来自动生成action的type
		initialState:{   
			name:'孙悟空',
			sex:"男"，
			address:'花果山'
		},  //state初始值
		reducers:{
			setName(state,action){
				//通过不同的action来指定对state的不同操作
				//state 是代理对象，可以之间使用,利用action对象来接收参数
				state.name = action.payload；
			}
		}
	}
})


//切片对象自动生成action对象，type:stu/setName
const {setName} = stuSlice.action       返回为函数


2、通过切片创建是store对象，接收配置对象
const store = configureStore(option:{
	reducer:{
		student :stuSlice.reducer,
		people :peoSlice.reducer,
	}
})

export defult store 

3、使用store
数据注入
<Provider store={store}>
	<APP/>
</Provider>

const APP = () =>{
	//接收全部state作为参数
	const student  = useSelector(state =>state.student)  //挑选  store中的名字
	//获取派发器
	const dispatch = useDispatch()
	const handName = () =>{
		//不用指定类型type，自动生成了
		dispatch(setName('网易宝'))
	}
}

## RTK Query
对state处理完后，需要将state和服务器，数据库向关联，进行数据请求与获取

### 1、创建APi对象
接收对象作为参数，返回APi对象
const studentAPi = createAPi（{
	reducerPath:'studentAPi'   //api唯一标识，不能和其他api或者reducer重复
	
	baseQuary:fetchBaseQuary({
		baseUrl:'http://a/b/')   //指定发送的基本信息，发送请求的工具
		prepareHeaders((headers,{getState}) =>{    //getState 获取所有的state
			coonst authorization = getState（）.auth.token
			headers.set('authorization','eeefefefe');
			return headers
		});      //请求头基本信息
	})
	
	tagType:['student','teacher']     //标签列表
	endpoints:(bulid)=>{
		//bulid是请求的构造器，通过build来设置请求信息
		return {
	        getStudent:bulid.quary（{
	            quary（id）{
	                return  'student/${id}'       //和baseUrl进行路径拼接
	            }
	            transformResponse  (baseQuaryReaultValue){
	                return baseQuaryReaultValue
	            }  				//转换请求到的数据格式
	            
	            keepUnusedDataFor:0;     设置数据缓存有效期，单位是秒  默认60s  ------改变自己默认统一缓存
	            
	            providesTags:['student']     //给函数添加标签
	            providesTags:（result,error,id）=>{
	            	return {type:'student',id}
	            }    //给函数添加标签
	            
	        }）, //配置查询的基本信息     返回对象{data:,isLoading,:staues:}
	        
	        deleteStudent:build.mutaion（{
	        	quary（id）{
	                return  {
	                	url:'student/${id}'       //和baseUrl进行路径拼接
	                	method:'delete'
	                }
	            }
	            invalidatesTags:['student']     //使带有student标签的函数失效，然后重新执行函数
	            
	            invalidatesTags:[{type:'student',id}]  //使带有student标签的函数失效，然后重新执行函数-------------改变他人缓存
	
	        }）  // 返回数组，第一个为操作触发器，第二个为结果集
	        
	        addStudent:build.mutaion（{
	        	quary（stu）{
	                return  {
	                	url:'student'       //和baseUrl进行路径拼接
	                	method:'post',
	                	body:{data:stu}
	                }
	            }
	
	        }）  // 返回数组，第一个为操作触发器，第二个为结果集
		}
		
	}        //endpoints 用来指定各种具体的功能
}）


api对象创建后，会根据endpoints自动生成对应的钩子函数
getStudent ---> useGetStudentQuary

### 2、导出
export const {useGetStudentQuary} = studentAPi

### 3、使用
使用方式有两种，一种是直接使用，另一种是作为store中的一个reducer使用

const store = configureStore({
	reducer:{
		[studentAPi.reducerPath]:studentAPi.reducerPath;
		auth: authSlice.render;
	}


	middleware:getDefaultMiddleware => 
		getDefaultMiddleware().concat(studentAPi.middleware)       ///中间件，增加数据缓存功能
})

setupListeners(store.dispatch)    //设置监听器，之后可以支持使用refetchOnFocus,refetchOnReconnect

### 接收参数
//接收参数,第二个参数接收一个对象进行个性配置
const obj =  useGetStudentQuary(id,{
 	selectFromResult:result =>result      //用来指定返回的结果
 	
 	pollingInterval:0   设置轮询间隔，单位为毫秒     重新发送请求    ------改变自己不同使用位置时的缓存  
 	skip:false   //跳过本次请求
 	
 	refachOnMountOrArgChange:false ,   //是否每次重新加载数据   是否使用缓存
 	
 	refatchOnFocus:true      //重新获取焦点时重新获取数据
 })

const [delStudent,result]  = useDeleteStuentMutation()
delStudent(id)

## React-Router  5
使用react编写的是单页面应用，将地址URL与组件进行映射，实现在客户端的组件的切换

<BrowserRouter>   //根组件  HashRouter作用类似，url的hash值映射组件
	<APP/>
</BrowserRouter>

### component
参数为类名
<Router exact  path='/'   component={Home}>      //将组件和URL进行映射     exact严格匹配    

component={Home}   通过component构建组件，会自动构建组件并向组件传递如下参数：
{
	match:       //匹配信息{
		isExact:    //exact为true则恒为true，exact为false则判断是否有后缀
		
		params:   //请求的参数    path='/student/:id'     自动匹配到/student/xxxx，match.params可以获取到 {id：XXX}
		
	}
	
	location:   //地址信息{
		search:  //获取地址中的参数  ?name='pp'
		state:    //页面之间进行传递数据，上一个页面传来的参数
	}
	
	history:    //控制页面的跳转{
		goback:
		goforward:
		push:    //页面跳转，相当于正常到达下一页面       可以用back回退
		replace:  {
			pathName:'studwnt/2'
			state:{哈哈哈}
		} //页面替换    back回退之前的页面
	}
}

//接收三个属性
const Home =（props）=>{
	props // history  location  match
	或者利用钩子函数
	useRouterMatch()
	useLocation()
	useHistory()
	useParams()
}
###  render
当父组件要给子组件传值时，同样需要props参数，此时不能再使用component

<Router exact  path='/'   render={（routeProps）=>{
		return <Home/>
	}
}>      //与component作用一致，参数为回调函数，返回值为渲染对象      不会自动传递三个属性

### children
与render类似
当参数为回调函数时，无论路径是否匹配，都会渲染组件
当参数为组件时，为jsx    <Home/>

<Routet path='student/'   children={() =>{}  /  <Home/> }>
等价于：<Routet path='student/'  >     <Home/>   </Router>


<Link  to='/'></Link>      与a标签类似，但是不会刷新页面，只会在客户端进行跳转   一定不要用a标签
点击刷新时或用普通url访问时，会向服务器发送请求，此时不经过react-router，会出现文件结构不一致，404错误，使用**HashRouter**或**修改服务器**的配置即可解决

<NavLink   exact  to='/'   activeClassName={css.active}> </NavLink>   增加了激活状态，可以显示当前组件link激活后的样式


### Prompt组件
当页面离开时，弹出提示
<Prompt    when={false}     message ='您即将离开该页面'/>    //当when为true时使用该组件

### Redirect组件
类似于history.replace,push作用，只是一个变种
const   Home = ()=>{
	return (
		<Redirect   push      from = '/'     to='/form'/>    //默认replace，可以改为push 
	)
}

### Switch组件
所有路由放入<Switch>中，一个Switch一次只会显示一个路由
<Switch>
	<Router></Router>
	<Router></Router>
	<Router></Router>
</Switch>


### 路由嵌套
<Router path='/'>

	<Router  path='/Student'></Router>

</Router>
或者 写在子组件内
useRouterMatch().pathName
const Home = () =>{
	return (<Router  path='/Student'></Router>
	)
}

## React-Router  6
新增Routes组件，作用与switch类似，但必须写，否则报错，switch可有可无
component, render , children变了，改为element
<Routes>
	<Router   path='/*'     element={<Home/>}>   </Router>     //默认严格匹配，加*表示取消默认匹配
</Routes>

###  参数变化
useParams()     //返回参数    同5
useMatch（'/'）  //路径匹配，相同则返回true，不同则返回null
useLocation（） //同5
const  nav = useNavigate()     //作用与useHistory 相似     用于页面跳转的函数,默认push方式，    nav('/about',option：{replace：true}) 

### 路由嵌套
<Routes>
	<Router   path='/'     element={<Home/>}>   
		<Router   path='student'     element={<Student/>}>   </Router> 
	</Router> 
</Routes>

const Home = () =>{
	return({
		<Outlet/>    //表示嵌套的子组件，路径匹配则显示，不匹配则不显示    -Student
	})
}

### Navigate组件
跳转到指定路由，和5中的Redirect类似
<Navigate   to='/student'  replace   state={} />   //默认时 push方式，加上replace则为replace方式
将state数据传递给to指定到的页面

### Navlink组件
激活样式，写法发生变化，style传入回调函数
<Navlink to='/'    style={(a)=>{
	console.log(a.isActive);
	return {background:red}
}}>   主页   </Navlink>
或者：
const navigate = useNavigate()
navigate('/',{replace:true})

## 权限控制

### 解决刷新自动退出问题
数据持久化------直接将数据从内存存到本地

localstorage.setItem('hahha':''')
localstorage.removeItem('hahha':''')
localstorage.getItem('hahha':''')

### 自动登出
由于token要过期，所有自动登出

const timer = setTimeout(()=>{},   time - Data.now()  )

return (() => clearTimeout(timer))    //避免开多个定时器


## HOOK
### useMemo

缓存函数执行结果，当函数执行少次且时间较长，可以保存结果，减少执行次数
const result = useMomo(() =>{
	return func()
},[])      //依赖项发生变化，会重新执行，【】表示只执行一次

当组件加载很慢时，也可以同样使用
const result = useMomo(() =>{
	return  <Demo/>
},[a])      //依赖项发生变化，会重新执行，【】表示只执行一次     {result}

### useImperativeHandle()
用来指定ref返回的值

const Some = react.forwardRef((props,ref)=>{    //通过forward参数ref，指定组件中的向外部暴露的具体dom，不知道无法对组件使用ref

	const inputRef = useRef();
	useImperativeHandle(ref,()=>{
		//返回的值会成为ref的值
		return changeValue(val){
			inputRef.current.value = val;    //通过返回函数，使外部不能直接操作内部dom
		}
	})
	
	return  <h1 ref={inputRef}></h1>
})


###  useLayoutEffect

组件挂载     -》     state改变     -》     useInsertionEffect     -》    Dom改变-》    useLayoutEffect (解决屏幕样式突变，造成闪的问题)    -》  屏幕绘制     -》      useEffect

### useDeferredValue
获取延迟值,每次获取前一次的值，所以state每次会执行两次
const [a,setA] = useState()
const defferedA = useDeferredValue(a)

当多个组件使用相同的state时，一个组件卡顿，可能导致全部卡顿，可用useDeferredValue和useMomo解决,正常的优先显示，延迟的使用延迟值

<Home/   props={a}>
<Demo/  peops={defferedA}>    //延迟的组件用延迟值，并用useMomo包装减少渲染次数

### useTransition
降低代码等级,当其他代码执行完后再执行
const [isPadding,startTransition] = useTransition()

startTransition（（） =》{
	setState('kk')
}）


## 时间周期函数 （旧）：
mount装载     unmount卸载

- 初始化阶段  由render触发 --初次渲染
	contribute()
    compentwillmount
    render（）
    compentdidmount    ===》执行一次   常用与做一些初始化的事，如开启定时器，发送网络请求，订阅消息
- 更新阶段 由组件内部this.setState()或父组件render触发
    shoudupdatacompent （）
    compentwillupdata   //将要更新
    render（）
    compentdidupdata （）   //更新完毕
- 卸载组件 由ReactDOM.unmountComponentAtNode()触发
	compentwllunmount  =====》一次    常用于做那个一些收尾的工作，关闭定时器，取消订阅消息

其他：compentwillreceiveprops    //第一次接收，不算

时间周期函数（新）：

废除了3个：
	compentwillmount
    compentwillupdata   //将要更新
    compentwillreceiveprops    //第一次接收，不算
新增2
 	getDerivedStateFromProps(props，states)    ：当state依赖props时可以调用，，用处罕见
	getSnapshotBeforeUpdate（）    ：在更新状态前，获取快照  罕见   
	
	compentdidupdata（preprops,prestate,snapshotValue）

## portal传送门:

组件会默认作为父组件的后代，渲染到页面中，但是有时会出现层级渲染的问题，如z-index失效
portal传送门，将组件渲染到指定位置，取消父子关系
ReactDOM.createportal(<div>ssss</div>,document.getElementById('root'))，将这个组件传送到root节点的位置，直接到最上层，减少父级传递

## React.context   参数传递问题
父组件向子组件传递值时，利用props逐层传递，会产生多余传递
为解决该问题，引发React.context 作为公共存储

- 第一步：创建公共内容
const TestContext = React.createContext({
	name:1111
})
- 第二步：获取公共共享内容   
  import   TestContext    from   './xxxx'
  <TestContext.Consumer>  //必须传入函数，参数为已经定义的共享值
    {(ctx)=>{
        return <div>
        	ctx.name
        </div>
    }}
  </TestContext.Consumer>

  或者
  const ctx = useContext(TestContext)    		//利用钩子函数获取   只能在函数组件中使用

- 第三步： 传入指定共享数据
  <A/>
  <TestContext.Provider  value={{name:33333}}>  	// 此处的value为想要的共享的值，包裹的子组件可以访问到该共享值(B)，未包裹时则无法访问到该值(A),优先访问最近的Context
       </B>
  </TestContext.Provider>

## 发送请求
- ajax：
- fetch：是Ajax的升级版，用于向服务器发送请求
	接收两个参数：一个是请求地址，另一个是请求信息（默认get方式）   
	返回promise类型
	fetch（"http://loaclhost:10.2/3/4/"，{
		method：'delete',
	}）
	.then(()  =>{})  成功时调用    
	.catch(() =>{}) ·失败时调用
- axios：


## 其他要点
this.weather1 = this.weather2.bind(this)   bind将函数weather1的this指向到weather2的实例对象上，并函数体和weather2一致（this拷贝）

类中可以直接进行赋值语句，a=1，不需要let,产生一个对象{a:1}

箭头函数自身无this，会去外部找，function则不会

连接数组[...arr1,arr2]
数组恒为true，不论是否为空，用.length处理

### 回调函数，自己创建的函数，自己没有调用，他人进行了调用

### 非受控组件：数据现用现取，不会保留原来状态  ref
受控组件：数据每次变化都要存储   onChange   (减少了ref的使用)   双向绑定（value+onchange+setState）


### 高级函数：接收参数为函数或返回值为一个函数     return ()=>{}
函数柯里化：函数调用继续返回函数，实现对接收参数的统一处理，一种函数的编码方式    map()
不柯里化：箭头函数  onchange={()=>{}}


### 为什么不建议使用index下标作为key的原因：
key是虚拟dom的唯一标识，每次更新dom，diff ing算法需要比较新旧dom的key，不一致时才会进行更新。
key需要保持不变性，数组的index会不断变换，会增加比较次数，如破坏顺序性操作，从头部插入数据时，会全部生成新dom，降低效率


react 借助的react-scripts进行处理命令 打包      依赖包包含大部分依赖（webpack,...）
npm react-scripts start

test --单元测试
eject  --将文件暴露出来，不可以回退

npm init -y   初始化 产生package.json 文件   形成可打包   配置文件


###  react渲染完成后，不会在进行值的修改，除非使用state，修改后，触发重新渲染

### 快捷方式 创建react组件结构
rcc
rsc
rsi


### 获取组件标签体
<Mycompontent> <>标签体</></Mycompontent>
props.children 获取标签体

### css样式

内联样式：style={{color:'red'}}

样式表： css文件   .p1{}

css模块：  
-  xxx.module.css      创建文件
- import classww from ',.xxx.module.css'      导入文件
- activeClassName={classwww.p1}          使用

###  <React.Frament >     </React.Frament >     ===      <></>     空容器

fontawesome   字体图标库

Strapi 前端页面进行自动创建服务器，进行建表，创建api，用于进行测试

async  变成异步         await  变成同步==（）.then()，解构promise数据