## 引言

前面写了多篇关于<跨平台文件在线预览解决方案>，不管使用pdf.js、LibreOffice，还是永中DCS，各自都有自己的优缺点，比如：pdf.js不支持双指放大缩小，LibreOffice加载缓慢，永中DCS收费等等。

* [跨平台（uni-app）文件在线预览解决方案](http://silianpan.cn/index.php/2019/10/25/uniapp_file_online_preview/)
* [跨平台文件在线预览解决方案（二）](http://silianpan.cn/index.php/2020/08/21/online_file_preview_2/)
* [跨平台文件在线预览解决方案（三）- LibreOffice vs OpenOffice](http://silianpan.cn/index.php/2020/09/09/file_preview_libreoffice/)

本文基于[uni-app](https://uniapp.dcloud.io/)平台实现了[Office文档在线预览原生插件sealOfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)，同时支持Android和IOS，欢迎下载使用~

[sealOfficeOnline插件下载使用地址](https://ext.dcloud.net.cn/plugin?id=3226)

<img src="http://silianpan.cn/wp-content/uploads/2020/10/h8chd-s1gok.gif" height="800" />


## 快速上手

[demo工程地址](https://github.com/silianpan/UniPlugin-Demo)

开发工具[HBuilderX](https://www.dcloud.io/hbuilderx.html)

### Step1. 下载demo工程，使用HBuilderX打开

### Step2. 下载本文插件

插件名称：[sealOfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)

下载插件解压放置到项目根目录`nativeplugins`下，如图：

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_8d84d41a362f0738223468efd3ef555c.jpg" width="280" />

### Step3. 使用插件

* 在vue或nvue组件中，导入插件

```js
var testModule = uni.requireNativePlugin("sealOfficeOnline")
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
```

## openFile方法参数说明

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

## Android预览效果

### 预览pdf

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_f8f91ec416985c5ee17a1ded21284686.jpg" height="800" />

### 预览txt

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_3ea464b119d1fb9bb3d9b35b1ba42fd9.jpg" height="800" />

### 预览docx

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_e7abb01d10d1e3aa495e17c415c78b5e.jpg" height="800" />

### 预览xlsx

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_9c50e83094411d75905c515232c86f38.jpg" height="800" />

### 预览pptx

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_1aa944cd4046400ab35598aaf0f038ab.jpg" height="800" />

## IOS预览效果

### 预览pdf

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_b3f93480acacf6c1762dd681be206e54.jpg" height="800" />

### 预览txt

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_559b79917a4f21c2872b988525d6954e.jpg" height="800" />

### 预览docx

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_ef4e4c360a3e0bdc1427fc351385a7a1.jpg" height="800" />

### 预览xlsx

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_681e2dc1fe1dc81899e0b5c9cffe929b.jpg" height="800" />

### 预览pptx

<img src="http://silianpan.cn/wp-content/uploads/2020/10/wp_editor_md_08b8bc5087438b4de626ec872b164001.jpg" height="800" />
