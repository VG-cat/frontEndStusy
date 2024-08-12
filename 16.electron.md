

# electron

桌面应用开发框架

```
 cnpm install -g electron
```

`main.js`   以下三个为主进程

```

const {app,BrowserWindow} = require('electron')
const path = require('path')
// 第一步：引入remote
const remote = require('@electron/remote/main');
// 第二步： 初始化remote
remote.initialize();

app.on('ready',function(){
    console.log('ffff');
    hm()

    global.a = 22
})


function hm(){
    let win = new BrowserWindow({
        width:300,
        height:560,
        title:'黑马计算器',
        webPreferences:{
            // 开启渲染进程能使用node,新版本还要将上下文设置为false
            nodeIntegration:true,
            contextIsolation:false,
            //旧版本只需增加这一配置,将新版本的三步去掉
            //enableRemoteModule:true
        }
    })

    win.loadURL(path.join(__dirname,'views/index.html'))

    win.webContents.openDevTools()

    win.on('close',function(event){
        // win = null,
        // app.quit()

        win.hide()
        win.setSkipTaskbar(true)
        event.preventDefault()
    })

    require('./menu')

    const createTray = require('./tary')
    createTray(win)

      // 第三步： 开启remote服务
    remote.enable(win.webContents);

}




```
`Menu.js`

```

const { Menu, dialog,BaseWindow } = require('electron')


// const win = new BaseWindow()

let template = [
    {
        label: '计算器',
        submenu: [{
            label: '退出',
            click: function (item,win,event) {
                console.log(222);
                dialog.showMessageBox({
                    title: '退出',
                    buttons: ['确定']
                }).then(response => {
                    console.log(response);
                    win.destroy()

                })
            }
        }]
    },
    {
        label: '放大',
        accelerator: process.platform === 'darwin' ? 'Alt+Cmd+I' : 'Alt+Shift+I',
        click: function () {
            console.log(222);
        }
    }
]

let menu = Menu.buildFromTemplate(template)

Menu.setApplicationMenu(menu)
```
`Tray.js`

```
const { Tray, Menu } = require('electron')
const path = require('path')

const createTray = function(win) {
    const tray = new Tray(path.join(__dirname,'./1.png'))

    tray.setToolTip('计算器')

    const menu = Menu.buildFromTemplate([
        {
            label: '退出',
            click: () => {
                console.log(22);
                tray.destroy()
                win.destroy()
            }
        }
    ])


    tray.setContextMenu(menu)

    tray.on('click', () => {
        if (win.isvisable) {
            win.hide()
            win.setSkipTaskbar(false)
        }else{
            win.show()
            win.setSkipTaskbar(true)
        }
    })

}


module.exports =  createTray

```
`a.js` 渲染进程

```
const { BrowserWindow, app } = require('@electron/remote') 
// const BrowserWindow = remote.BrowserWindow;
const remote = require('@electron/remote/main');

const btn = document.querySelector('#btn')

btn.onclick = () => {
    var win = new BrowserWindow({ width: 800, height: 600 });
    win.loadURL('https://github.com');
    console.log(remote.getGlobal('a'));
}

```


## 主进程与渲染进程通信

主进程：main.js以及直接在main.js中引入的js

```
ipcMain.on(channel,listener)   //channel为事件频道(自定义事件)，listener为响应ipcRender.send()的回调函数

event.sender.send()   //监听渲染进程信息后，在listener回调函数中发送信息(被动)

win.webContent.on('did-finish-load',()=>{
	win.webContent.send(channel,data)      //主进程先手向渲染进程发送信息（主动）
})
```



渲染进程：其他每一个页面均会有一个进程

```
ipcRender.on(channel,listener)   //同上

ipcRender.send(channel,data)     //向主进程发送信息
```

```
const {ipcRender} = require('electron')

ipcRender.send('nihao',222)     



//main.js
const {ipcMain} = require('electron')

ipcMain.on('nihoa',(event,msg)=>{
	log(msg)
	event.sender.send('nihao333',333)
})
```



## 渲染进程间通信

- 借助localstorage或sessionstorage来实现数据共享
- 主进程定义共享变量，渲染进程借助remote模块来操作

 ```
 global.a = '33'
 
 
 //渲染进程.js
 
 const remote = require('electron').remote
 
 const {BrowserWindow} = require('electron').remote   //在渲染进程中引入，只能主进程使用的模块
 
 log(remote.getGlobal('a'))
 ```



## 打包

打包为可执行文件

```
npm i electron-package
```



打包为安装包

```
npm i electron-builder
```



