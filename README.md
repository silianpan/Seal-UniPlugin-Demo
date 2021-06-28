## 引言

前面写了多篇关于<跨平台文件在线预览解决方案>，不管使用pdf.js、LibreOffice，还是永中DCS，各自都有自己的优缺点，比如：pdf.js不支持双指放大缩小，LibreOffice加载缓慢，永中DCS收费等等。

* [跨平台（uni-app）文件在线预览解决方案](http://silianpan.cn/index.php/2019/10/25/uniapp_file_online_preview/)
* [跨平台文件在线预览解决方案（二）](http://silianpan.cn/index.php/2020/08/21/online_file_preview_2/)
* [跨平台文件在线预览解决方案（三）- LibreOffice vs OpenOffice](http://silianpan.cn/index.php/2020/09/09/file_preview_libreoffice/)
* [跨平台文件在线预览解决方案（四）-Android和IOS原生插件](http://silianpan.cn/index.php/2020/10/17/file_preview_uni_native/)
* [跨平台文档在线预览解决方案（五）-水印、防复制、在线编辑等](http://silianpan.cn/index.php/2021/03/14/file_preview_5/)

本文基于[uni-app](https://uniapp.dcloud.io/)平台实现了[Office文档在线预览原生插件Seal-OfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)，同时支持Android和iOS，欢迎下载使用~,

另外单独抽离了图片预览和视频播放原生插件：Seal-ImageVideo，根据自己情况，选择使用~，[Seal-ImageVideo插件下载使用地址](https://ext.dcloud.net.cn/plugin?id=5478)，另附[Github文档地址](https://github.com/silianpan/Seal-UniPlugin-Demo/blob/main/README-ImageVideo.md)

[Seal-OfficeOnline插件下载使用地址](https://ext.dcloud.net.cn/plugin?id=3226)

<div class="half">

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/2c7eada0-15dc-11eb-b680-7980c8a877b8.gif" height="600" style="height:600px" />

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/38c046f0-102d-11eb-b244-a9f5e5565f30.gif" height="600" style="height:600px" />

</div>

## 注意事项

* ### `manifest.json->App模块配置不要勾选Android X5 Webview`
* ### 不要同时使用其他任何预览插件，否则，会引起打包冲突
* ### 如遇预览失败，（1）把APP卸载，重新打基座运行，第一次运行会自动安装X5插件；（2）或者换手机（华为或小米）进行测试

## 问题解决

### 问题一：

> 问题描述：真机测试有4部手机测试， 小米MAX3、坚果pro2 、华为P20 这个三个手机测试预览都能成功，小米redmi K30 预览 就不成功，
> "canLoadVideo": false, "canLoadX5": false, "coreVersion": 0, "sdkVersion": 43967, "canLoadX5FirstTimeThirdApp": false, "isCoreInited": true
> X5内核没有加载成功，反复卸载APP和重新打包基座好几次了，X5内核都是没有加载成功，这个问题该如何解决

后面想了一下可能是CPU支持类型的问题，

**解决方案：manifest.json -APP常用其他设置 -支持CPU类型 armeabi-v7a arm64-v8a 两个都勾选的**，

重新打一下自定义基座 问题就解决了

## 快速上手

[demo工程地址](https://github.com/silianpan/Seal-UniPlugin-Demo) 或在右上角直接下载示例工程

开发工具：[HBuilderX](https://www.dcloud.io/hbuilderx.html)

### Step1. 下载demo工程，使用HBuilderX打开

### Step2. 下载本文插件

插件名称：[Seal-OfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)

点击右上角`试用`或者`购买`，选择你的云打包插件

### Step3. 选择`manifest.json->App原生插件配置`中加载云端插件

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/eb31ccf0-15dc-11eb-8ff1-d5dcf8779628.png" width="600" style="width:600px" />

### Step4. 使用插件

* 在vue或nvue组件中，导入插件

```js
var testModule = uni.requireNativePlugin("Seal-OfficeOnline")
```

* **openFile**方法：支持Android和IOS，预览Office文件，支持如下格式：pdf、txt、doc、docx、xls、xlsx、ppt、pptx等
* **OpenFileBS**方法：只支持Android，打开在线文档，支持Excel在线编辑，PPT全屏浏览，查看最近打开文件，发送分享文档，采用其他应用打开等

```js
// 平台支持：Android和IOS
// Android支持以下全部参数
// IOS支持：url，title，topBarBgColor，topBarTextColor
// 方式一：直接在openFile接口中传递在线url
testModule.openFile({
	url: 'http://113.62.127.199:8090/fileUpload/1.xlsx', // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
	isTopBar: true, // 是否显示顶栏，默认为：true（显示）
	title: 'Office文档在线预览', // 顶栏标题，默认为：APP名称
	topBarHeight: 100, // 顶栏高度，默认为actionBarSize
	topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#177cb0（靛青）
	topBarTextColor: '#cf1322', // 顶栏标题文字颜色，默认为：#FFFFFF（白色）
	topBarTextLength: 12, // 顶栏标题文字长度，默认为：12
	isBackArrow: true, // 是否显示返回按钮，默认为：true（显示）
	fileType: 'xlsx', // 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型
	fileName: '1', // 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType
	initTitle: '你好，世界', // 初始化插件动画标题，默认：'插件初始化'
	initBody: '怎么了', // 初始化插件动画内容，默认：'加载中...'
	isDeleteFile: true, // 退出是否删除缓存的文件，默认为true（删除缓存文件）
});
// 方式二：先调用uni-app接口uni.downloadFile下载文件，然后获取本地文件绝对路径，传递到openFile接口url参数中
openOnlineFile2(fileName) {
	// 调用uni.downloadFile接口下载文件
	uni.downloadFile({
		url: 'http://113.62.127.199:8090/fileUpload/' + fileName,
		success: (res) => {
			if (res.statusCode === 200) {
				// 传递本地文件绝对路径，res.tempFilePath的前缀是_doc，而实际目录为doc，没有下划线_，所以要substr取子串
				// const url = '/sdcard/Android/data/APP包名/apps/APPID/' + res.tempFilePath.substr(1)
				// 可以通过一下方式获取文件绝对路径
				uni.saveFile({
					// 需要保存文件的临时路径
					tempFilePath: res.tempFilePath,
					success: (resSave) => {
						const savedFilePath = resSave.savedFilePath
						// 转换为绝对路径
						const url = plus.io.convertLocalFileSystemURL(savedFilePath)
						// 预览本地文件
						testModule.openFile({
							url: url,
						});
					}
				});
			}
		}
	});
},

// 图片预览，支持jpg、jpeg、png、bmp、jpg、gif等多种常用图片格式
// 图片可以来源于列表或九宫格，传递给imageUrls数组
const url = 'http://113.62.127.199:8090/fileUpload/'
testModule.openFile({
	imageUrls: [ // 图片url数组，此参数优先于文档预览
		url + '1.jpg',
		url + '1.jpeg',
		url + '1.png',
		url + '1.bmp',
		url + '1.gif'
	],
	imageCurrentIndex: 0, // 当前点击图片在imageUrls中的下标，从0开始，默认为0
	imageIndexType: 'number' // 图片底部指示器类型，默认为'dot'，可选：'number':数字；'dot':点
})

// 视频播放，支持市面上几乎所有的视频格式，包括mp4, flv, avi, 3gp, webm, ts, ogv, m3u8, asf, wmv, rm, rmvb, mov, mkv等18种视频格式
// 功能包括：全屏播放、锁屏、分享、画面比例调节、左边上下滑动调节亮度，右边上下滑动调节音量等
// 支持Android和IOS
testModule.openFile({
	videoUrl: 'http://113.62.127.199:8090/fileUpload/1.mp4', // 视频在线url，此参数优先于图片预览和文档预览
})

// QQ浏览服务打开在线文档，支持Excel在线编辑，PPT全屏浏览，查看最近打开文件，发送分享文档，采用其他应用打开等
// 平台支持：Android，支持以下全部参数
testModule.openFileBS({
	url: 'http://113.62.127.199:8090/fileUpload/1.xlsx', // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
	topBarBgColor: '#3394EC',// 顶栏背景颜色，默认为：#177cb0（靛青）
	fileType: 'xlsx', // 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型
	fileName: '1', // 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType
	initTitle: '你好，世界', // 初始化插件动画标题，默认：'插件初始化'
	initBody: '怎么了', // 初始化插件动画内容，默认：'加载中...'
	isDeleteFile: true, // 退出是否删除缓存的文件，默认为true（删除缓存文件）
})

// 获取内核信息，用于调试
const coreInfo = testModule.getX5CoreInfo()
// 返回
{
	'isCoreInited': false, // 内核是否加载
	'coreVersion': 0, // 内核版本
	'sdkVersion': 43967, // sdk版本
}
```

### Step5. 调试

* 制作自定义调试基座：在开发工具中点击“运行到手机或模拟器-》制作自定义调试基座”
* 选择自定义调试基座：然后“运行到手机或模拟器-》基座运行选择-》自定义调试基座”
* 连接真机，运行调试

## openFile方法参数说明

支持打开在线文档，本地文档

### url

url参数支持如下三种地址方式：

* 文件网络地址，如：http://113.62.127.199:8090/fileUpload/1.xlsx

* 手机本地文件地址，如：/data/user/0/APP包名/files/1.xlsx

* 文件名，如：1.xlsx，访问默认目录文件，默认目录为：/data/user/0/APP包名，如：com.HBuilder.UniPlugin

**注意**：手机本地地址目录需要有权限访问

### isTopBar

isTopBar：是否显示顶栏，默认为：true（显示）；显示时，向上滑动顶栏会自动隐藏

### title

title：顶栏标题（isTopBar为true时有效），默认为：APP名称

### topBarHeight

topBarHeight：顶栏自定义高度（isTopBar为true时有效），类型为正整数，默认为：actionBarSize

### topBarBgColor

topBarBgColor：顶栏背景颜色（isTopBar为true时有效），默认为：#177cb0（靛青）

### topBarTextColor

topBarTextColor：顶栏文本颜色（isTopBar为true时有效），默认为：#FFFFFF（白色）

### topBarTextLength
topBarTextLength：顶栏标题文字长度（isTopBar为true时有效），默认为：12

### isBackArrow

isBackArrow：是否显示返回按钮（isTopBar为true时有效），默认为：true（显示）

### fileType

fileType：可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型

### fileName

fileName：指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType

### initTitle

initTitle：初始化插件动画标题，默认：'插件初始化'

### initBody

initBody：初始化插件动画内容，默认：'加载中...'

### isDeleteFile
isDeleteFile：退出是否删除缓存的文件，默认为true（删除缓存文件）

### imageUrls
imageUrls：图片url数组，此参数优先于文档预览；长按图片底部弹出保存图片菜单，保存图片至相册

### imageCurrentIndex
imageCurrentIndex：当前点击图片在imageUrls中的下标，从0开始，默认为0

### imageIndexType
imageIndexType: 图片底部指示器类型，默认为'dot'，可选：'number':数字；'dot':点

### videoUrl
videoUrl：视频在线url，此参数优先于图片预览和文档预览

### 事件监听：文件关闭事件SealEventCloseFile，返回文件名和文件路径

注意：isDeleteFile为false时，路径下的文件才存在

```js
onLoad() {
	plus.globalEvent.addEventListener('SealEventCloseFile', function(e) {
		modal.toast({
			// 返回参数{fileName:'xx', filePath:'xx'}
			message: "SealEventCloseFile文件关闭事件：" + JSON.stringify(e),
			duration: 3
		})
	})
},
```


## openFileBS方法参数说明

QQ浏览服务打开在线文档

* 支持QQ浏览器在线编辑、全屏播放、阅读模式等
* 支持QQ浏览器打开46种文件格式文件
* 查看最近打开文件
* 发送分享文档
* 采用其他应用打开等

同上，参数支持：url，topBarBgColor，fileType，fileName，initTitle，initBody，isDeleteFile

## 后续计划

* 1. 添加水印
* 2. 支持小窗口模式
* 3. 支持右上角自定义菜单

## Android预览效果

### 预览docx

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/9dec53b0-1024-11eb-b244-a9f5e5565f30.jpg" height="600" style="height:600px" />

### 预览pptx

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/a8316a90-1024-11eb-8bd0-2998ac5bbf7e.jpg" height="600" style="height:600px" />

### 预览xlsx

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/b302bbe0-1024-11eb-8ff1-d5dcf8779628.jpg" height="600" style="height:600px" />

### 预览pdf

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/baaae0c0-1024-11eb-9dfb-6da8e309e0d8.jpg" height="600" style="height:600px" />

### 预览txt

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/c0676240-1024-11eb-81ea-f115fe74321c.jpg" height="600" style="height:600px" />


## iOS预览效果

### 预览docx

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/c590cb80-1024-11eb-8ff1-d5dcf8779628.jpg" height="600" style="height:600px" />

### 预览pptx

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/cab1d050-1024-11eb-8a36-ebb87efcf8c0.jpg" height="600" style="height:600px" />

### 预览xlsx

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/cff6d7e0-1024-11eb-b680-7980c8a877b8.jpg" height="600" style="height:600px" />

### 预览pdf

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/dd023ab0-1024-11eb-b997-9918a5dda011.jpg" height="600" style="height:600px" />

### 预览txt

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/e28eab80-1024-11eb-8a36-ebb87efcf8c0.jpg" height="600" style="height:600px" />

转载请注明：[我的技术分享](http://silianpan.cn) » [跨平台文件在线预览解决方案（四）-Android和IOS原生插件](http://silianpan.cn/index.php/2020/10/17/file_preview_uni_native/)
