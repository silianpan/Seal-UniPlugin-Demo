## 引言

前面写了多篇关于<跨平台文件在线预览解决方案>，不管使用pdf.js、LibreOffice，还是永中DCS，各自都有自己的优缺点，比如：pdf.js不支持双指放大缩小，LibreOffice加载缓慢，永中DCS收费等等。

* [跨平台（uni-app）文件在线预览解决方案](http://silianpan.cn/index.php/2019/10/25/uniapp_file_online_preview/)
* [跨平台文件在线预览解决方案（二）](http://silianpan.cn/index.php/2020/08/21/online_file_preview_2/)
* [跨平台文件在线预览解决方案（三）- LibreOffice vs OpenOffice](http://silianpan.cn/index.php/2020/09/09/file_preview_libreoffice/)

本文基于[uni-app](https://uniapp.dcloud.io/)平台实现了[Office文档在线预览原生插件Seal-OfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)，同时支持Android和iOS，欢迎下载使用~

[Seal-OfficeOnline插件下载使用地址](https://ext.dcloud.net.cn/plugin?id=3226)

<div class="half">

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/2c7eada0-15dc-11eb-b680-7980c8a877b8.gif" height="600" style="height:600px" />

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/38c046f0-102d-11eb-b244-a9f5e5565f30.gif" height="600" style="height:600px" />

</div>

## 快速上手

[demo工程地址](https://github.com/silianpan/UniPlugin-Demo) 或在右上角直接下载示例工程

开发工具：[HBuilderX](https://www.dcloud.io/hbuilderx.html)

### Step1. 下载demo工程，使用HBuilderX打开

### Step2. 下载本文插件

插件名称：[Seal-OfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)

下载插件解压放置到项目根目录`nativeplugins`下，如图：

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/8c698370-102d-11eb-9dfb-6da8e309e0d8.png" height="300" style="height:300px" />

### Step3. 选择`manifest.json->App原生插件配置`中加载云端插件

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/eb31ccf0-15dc-11eb-8ff1-d5dcf8779628.png" width="600" style="width:600px" />

### Step4. 使用插件

* 在vue或nvue组件中，导入插件

```js
var testModule = uni.requireNativePlugin("Seal-OfficeOnline")
```

* 使用openFile方法预览Office文件，支持如下格式：pdf、txt、doc、docx、xls、xlsx、ppt、pptx

```js
testModule.openFile({
	url: 'http://113.62.127.199:8090/fileUpload/1.xlsx',
	topBarBgColor: '#3394EC',
	topBarTextColor: '#FFFFFF',
	title: 'Office文档在线预览',
	isBackArrow: false,
	fileType: 'xlsx',
	fileName: '1'
});

// QQ浏览服务打开在线文档，支持Excel在线编辑，PPT全屏浏览，查看最近打开文件，发送分享文档，采用其他应用打开等
testModule.openFileBS({
	url: 'http://113.62.127.199:8090/fileUpload/1.xlsx'
});
```

## openFile方法参数说明

支持打开在线文档，本地文档

### url

url参数支持如下三种地址方式：

* 文件网络地址，如：http://113.62.127.199:8090/fileUpload/1.xlsx

* 手机本地文件地址，如：/data/user/0/com.HBuilder.UniPlugin/files/1.xlsx

* 文件名，如：1.xlsx，访问默认目录文件，默认目录为：/data/user/0/com.HBuilder.UniPlugin

**注意**：手机本地地址目录需要有权限访问

### title

title表示顶栏文本，默认为：SealOfficeOnline

### topBarBgColor

topBarBgColor表示顶栏背景颜色，默认为：#177cb0（靛青）

### topBarTextColor

topBarTextColor表示顶栏文本颜色，默认为：#FFFFFF（白色）

### isBackArrow

isBackArrow表示是否显示返回按钮，默认为：true（显示）

### fileType

fileType表示可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型

### fileName

fileName可以指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType

## openFileBS方法参数说明

QQ浏览服务打开在线文档

* 支持QQ浏览器在线编辑、全屏播放、阅读模式等
* 支持QQ浏览器打开46种文件格式文件
* 查看最近打开文件
* 发送分享文档
* 采用其他应用打开等

### url

url参数支持如下三种地址方式：

* 文件网络地址，如：http://113.62.127.199:8090/fileUpload/1.xlsx

* 手机本地文件地址，如：/data/user/0/com.HBuilder.UniPlugin/files/1.xlsx

* 文件名，如：1.xlsx，访问默认目录文件，默认目录为：/data/user/0/com.HBuilder.UniPlugin

**注意**：手机本地地址目录需要有权限访问

## 后续计划

* 支持文件加载完成事件处理
* 支持文件关闭事件处理
* 其他事件处理

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
