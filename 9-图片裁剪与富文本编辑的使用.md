#图片裁剪cropper



![](.\images\img\Snipaste_2024-04-25_13-56-48.png)

## css

```
<link rel="stylesheet" href="assets/lib/cropper/cropper.css">

<link rel="stylesheet" href="assets/lib/layui/css/layui.css">
<style>
        .container {
            width: 500px;
            height: 500px;
        }

        .container img {
            max-width: 100%;
        }
    
        .w100 {
            width: 100px;
            height: 100px;
            overflow: hidden;
        }
    
        .w50 {
            width: 50px;
            height: 50px;
            overflow: hidden;
        }
</style>
```

## html

```
  			<div class="container">
                <img id="image" src="assets/images/sample.jpg">
            </div>


            <div>
                <div>
                    <div class="img-preview w100"></div>
                    <p class="size">100 X 100</p>
                </div>
                <div>
                    <div class="img-preview w50"></div>
                    <p class="size">50 X 50</p>
                </div>
                <div class="layui-btn-container" style="margin-top:30px">
                    <input type="file" id="file" accept="image/png,image/jpg" style="display:none;">
                    <button type="button" class="layui-btn layui-btn-normal">上传</button>
                    <button type="button" class="layui-btn ok">确定</button>

                </div>
            </div>
```
## js
```
   <!-- 按顺序 -->
    <script type="text/javascript" src="assets/lib/jquery.js"></script>
    <script src="assets/lib/cropper/Cropper.js"></script>
    <script src="assets/lib/cropper/jquery-cropper.js"></script>
    
    //组件事件
     <script type="text/javascript" src="assets/lib/layui/layui.all.js"></script>
```



## js使用
```
   	var $image = $('#image');   //获取图片对象

    // 通过jquery Dom 的cropper方法初始化
    $image.cropper({
        aspectRatio: 16 / 16,
        viewMode: 0,
        minContainerWidth: 500,  //裁剪区域大小，会改变容器大小
        minContainerHeight: 500,
        dragMode: 'move',
        preview: document.querySelectorAll('.img-preview')    //预览显示的dom
    });
    // 可以通过Dom对象的data的cropper属性获取初始化后获取Cropper.js实例
    var cropper = $image.data('cropper');

    
    //上传按钮，选择文件
    $('.layui-btn-normal').on('click', (e) => {
        $('#file').click()
    })

	//监听change 函数
    $('#file').on('change', (e) => {
        console.log(e);
        const files = e.target.files
        if (files.length === 0) {
            return
        } else {
            const newImgUrl = URL.createObjectURL(files[0])   //它会生成一个可以在浏览器中访问的唯一的本地URL。临时使用
            $image.cropper('destroy');   //先销毁旧的
            $image.attr('src', newImgUrl)   //创建新的
            $image.cropper({
                aspectRatio: 16 / 16,
                viewMode: 0,
                minContainerWidth: 500,
                minContainerHeight: 500,
                dragMode: 'move',
                preview: document.querySelectorAll('.img-preview')
            });
        }
    })

    $('.ok').on('click', (e) => {
        const dataurl = $image.cropper('getCroppedCanvas', {
            width: 100, height: 100
        }).toDataURL('image/png')    //返回base64编码的url,可以直接使用，数据库text类型存储

        // console.log(dataurl);
        $.ajax({
            method: "post",
            url: 'http://127.0.0.1:8081/api/avatatInfo',
            data: {
                username: localStorage.getItem('username'),
                avatar: dataurl
            },

            success: (res) => {
                layer.msg(res.message)
                console.log(res);         
                window.parent.getInfo()   //调用父类的方法

            },
            error: (res) => {
                console.log(res);
            }
        })
    })

```

## blob对象类型
也可以直接放到src里使用

toBlob 是 Canvas API 的一部分，它允许你将 canvas 元素的内容生成为一个 Blob 对象
Blob 对象可以是图片、视频、音频等二进制文件，
将 canvas 上的图像导出为图片格式时，通常会使用 toBlob 方法。这个方法接收一个回调函数，该函数在 blob 创建完成后执行。

toBlob 是异步的，这意味着在 toBlob 的回调函数之外的代码会在 blob 创建完成之前执行。因此，如果你需要使用 blob 对象，应该在回调函数内部进行操作。



var imageUrl = URL.createObjectURL(blob);        //也可以直接赋值给src使用,返回一个对象类型

或者window.url.createObjectUrl(file)

```
 		$image.cropper('getCroppedCanvas', {
            width: 100, height: 100
        }).toBlob((blob) => {
            fd.append('avatar', blob)
            console.log(blob);
            fd.forEach((k, v) => {
                console.log(k + '--------' + v);
            })
            $.ajax({
                method: 'post',
                url: "http://127.0.0.1:8081/api/publishArtical",
                contentType: false,
                processData: false,
                data: fd,
                success: (res) => {
                    console.log(res);
                    if (res.status === 200) {
                        layui.layer.msg(res.message)
                        location.href = './artlist'
                    }

                }
            })


        }, 'image/png')
```

## toDataURL

toDataURL 是 HTML5 canvas 元素的一个方法，它将 canvas 的内容生成为一个 Base64 编码的DataURL 字符串。这个字符串可以直接用作图片的 src 属性值，用于在网页上显示图片，或者用于其他需要图片数据的地方。

当你调用 toDataURL('image/png') 方法时，它会返回一个 MIME 类型为 image/png 的 DataURL 字符串。如果你不指定类型，默认会是 image/jpeg。

#富文本编辑tinymce
![](.\images\img\Snipaste_2024-04-25_13-57-33.png)

## css

## html
```
 		<div class="layui-input-block">
               <textarea name="content" id="texteditor"></textarea>   
        </div>
```

## js

```

    <script type="text/javascript" src="assets/lib/jquery.js"></script>
    <script type="text/javascript" src="assets/lib/layui/layui.all.js"></script>
    <script type="text/javascript" src="assets/lib/tinymce/tinymce.min.js"></script>
    <script type="text/javascript" src="assets/lib/tinymce/tinymce_setup.js"></script>
```

## js使用
```
initEditor()

const fd = new FormData($('form')[0])
fd.set('content', tinymce.get("texteditor").getContent())   //获取内容
```


# 后端存文件multer
以存储前端传来的blob图片文件为例

const multer = require('multer');


1、定义服务器保存文件的位置，uploads文件夹下，同时定义了文件名

const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'upload/');
    },
    filename: (req, file, cb) => {
        cb(null, file.fieldname + '-' + Date.now() + path.extname(file.originalname));
    }
});

2、配置中间件

const upload = multer({ storage: storage });

3、使用

req中会增加一个file属性，定义了存储路径后会增加path属性
avatar为前端传来formdata的键名

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
