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

<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询。</span>

<div class="half">

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/2c7eada0-15dc-11eb-b680-7980c8a877b8.gif" height="600" style="height:600px" />

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/38c046f0-102d-11eb-b244-a9f5e5565f30.gif" height="600" style="height:600px" />

</div>

## 一、注意事项

* ### `manifest.json->App模块配置不要勾选Android X5 Webview`

* ### 不要同时使用其他任何预览插件，否则，会引起打包冲突

* ### 如遇预览失败，（1）把APP卸载，重新打基座运行，第一次运行会自动安装X5插件；（2）或者换手机（华为或小米）进行测试

## 二、问题解决

### 问题一：

> 问题描述：真机测试有4部手机测试， 小米MAX3、坚果pro2 、华为P20 这个三个手机测试预览都能成功，小米redmi K30 预览 就不成功，
> "canLoadVideo": false, "canLoadX5": false, "coreVersion": 0, "sdkVersion": 43967, "canLoadX5FirstTimeThirdApp": false, "isCoreInited": true
> X5内核没有加载成功，反复卸载APP和重新打包基座好几次了，X5内核都是没有加载成功，这个问题该如何解决

后面想了一下可能是CPU支持类型的问题，

**解决方案：manifest.json -APP常用其他设置 -支持CPU类型 armeabi-v7a arm64-v8a 两个都勾选的**，

重新打一下自定义基座 问题就解决了。<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询。</span>



## 三、快速上手

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
const sealOfficeOnlineModule = uni.requireNativePlugin("Seal-OfficeOnline")
```

* **openFile**方法（推荐）：支持Android和IOS，预览Office文件，支持如下格式：pdf、txt、doc、docx、xls、xlsx、ppt、pptx、epub等
* **openFileWPS**方法（推荐）：采用本机WPS客户端预览或编辑文档，支持pdf、txt、doc、xls、ppt等多种文件格式。
* **checkWps**方法：检查本机WPS客户端是否已经安装。
* **openFileBS**方法：仅支持Android，打开在线文档，支持Excel在线编辑，PPT全屏浏览，查看最近打开文件，发送分享文档，采用其他应用打开等
* **getX5CoreInfo**方法（调试）：获取内核安装信息，用于调试

**<span style="color:red">接口使用方法，参考如下章节《四、使用方法》</span>**

### Step5. 调试

* 制作自定义调试基座：在开发工具中点击“运行到手机或模拟器-》制作自定义调试基座”
* 选择自定义调试基座：然后“运行到手机或模拟器-》基座运行选择-》自定义调试基座”
* 连接真机，运行调试

## 四、使用方法

### 1、组件嵌入预览

组件名：Seal-OfficeOnline

> 组件嵌入预览，是采用nvue原生组件的方式，嵌入页面预览，用于页面的局部文档预览。
>
> 在线内核、离线内核均可使用，也支持自定义水印。
>
> 注意：要采用nvue原生组件方式，不可以采用vue组件方式。
>
> 支持平台：Android、IOS

新建组件：seal-officeonline-component.nvue

```vue
<template>
	<div><Seal-OfficeOnline :options="options" style="width:600rpx;height:800rpx;border: 1px solid red;"></Seal-OfficeOnline></div>
</template>

<script>
export default {
	data() {
		return {
			options: {}
		}
	},
	onLoad(params) {
		console.log('params', params)
		this.options = {
			// 文档预览，传递url
			url: params.url,
			waterMarkText: '你好，世界\n准备好了吗？时刻准备着',
		}
	}
};
</script>
```

### 2、本地（离线）文档预览

使用接口：openFIle，参数参考章节《五、openFile接口参数说明》

内核插件下载地址：[地址一](http://silianpan.cn/upload/2022/09/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release-c5d6cd8bba8843b780e5943f9806c028.tbs)、[地址二](https://tbs.imtt.qq.com/release/x5/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release.tbs)

> 本地（离线）文档预览，是离线预览手机本地文档的方式。
>
> 前提，需要传递coreUrl参数（内核在线地址）。或者将离线内核安装包本地路径传递到接口参数coreLocalPath中，也可以先从在线地址下载到本地，获取本地路径传递到参数中。
>
> 同样，文档也可以下载到手机本地，获取文档本地路径，传递到接口url参数中。
>
> 注意：添加参数`isDeleteFile: false`，否则退出预览，文件删除
>
> 支持平台：Android

```javascript
// 方法一、传递coreUrl参数
openOfflineFile(fileUrl) {
    sealOfficeOnlineModule.openFile(
        {
            url: url,
            isDeleteFile: false,
            installOfflineCore: true, // 是否离线安装内核
            coreUrl: 'https://tbs.imtt.qq.com/release/x5/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release.tbs', // 离线内核包在线地址
        },
        res => {
            uni.showModal({
                content: '打开本地文档事件结果：' + JSON.stringify(res)
            });
        }
    );
}

// 方法二、传递coreLocalPath参数
/**
* 应用刚进入或页面刚进入，初始化下载内核到本机，获取本地路径
*/
downloadCoreToLocal() {
    uni.showLoading({
        title: '正在下载安装内核，请稍后~'
    });
    const coreUrl = 'https://tbs.imtt.qq.com/release/x5/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release.tbs';
    // const coreUrl = 'http://silianpan.cn/upload/2022/09/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release-c5d6cd8bba8843b780e5943f9806c028.tbs'
    uni.downloadFile({
        url: coreUrl,
        success: res => {
            if (res.statusCode === 200) {
                // 保存到应用目录
                uni.saveFile({
                    tempFilePath: res.tempFilePath,
                    success: resSave => {
                        uni.hideLoading();
                        // 获取本地路径
                        this.coreLocalPath = resSave.savedFilePath;
                    }
                });
            }
        }
    });
},
            
/**
* 打开本地文档
* @param {Object} fileUrl 文档url
*/
openOfflineFile(fileUrl) {
    // 方式一：直接传递本机文件绝对路径
    // 注意：添加参数`isDeleteFile: false`，否则退出预览，文件删除
    // 比如：/storage/sdcard0/Android/data/com.seal.uniplugin/files/file/test.docx
    // sealOfficeOnlineModule.openFile({
    // 		url: '/storage/sdcard0/Android/data/com.seal.uniplugin/files/file/test.docx',
    // 		isDeleteFile: false,
    // 		installOfflineCore: true, // 是否离线安装内核
    // 		coreLocalPath: this.coreLocalPath, // 离线安装内核本地路径
    // 	},
    // 	res => {
    // 		uni.showModal({
    // 			content: '打开本地文档事件结果：' + JSON.stringify(res)
    // 		});
    // 	});

    // 方式二：先下载文件保存到手机目录，再获取文件绝对路径
    uni.showLoading({
        title: '正在下载文件，请稍后~'
    });
    uni.downloadFile({
        url: fileUrl,
        success: res => {
            if (res.statusCode === 200) {
                // 直接传递本地文件地址
                // 传递本地文件绝对路径，res.tempFilePath的前缀是_doc，而实际目录为doc，没有下划线_，所以要substr取子串
                // const url = '/storage/sdcard0/Android/data/APP包名/apps/APPID/' + res.tempFilePath.substr(1)
                // 可以通过以下方式获取文件绝对路径
                uni.saveFile({
                    // 需要保存文件的临时路径
                    tempFilePath: res.tempFilePath,
                    success: resSave => {
                        uni.hideLoading();
                        const savedFilePath = resSave.savedFilePath;
                        // 转换为绝对路径
                        const url = plus.io.convertLocalFileSystemURL(savedFilePath);
                        console.log('tempFilePath', res.tempFilePath);
                        console.log('savedFilePath', savedFilePath);
                        console.log('url', url);
                        // 预览本地文件
                        sealOfficeOnlineModule.openFile(
                            {
                                url: url,
                                isDeleteFile: false,
                                installOfflineCore: true, // 是否离线安装内核
                                coreLocalPath: this.coreLocalPath // 离线安装内核本地路径
                            },
                            res => {
                                uni.showModal({
                                    content: '打开本地文档事件结果：' + JSON.stringify(res)
                                });
                            }
                        );
                    }
                });
            }
        }
    });
},
```

### 3、在线文档预览

使用接口：openFIle

> 在线文档预览，是内核初次采用在线安装，预览在线文档url地址的方式。
>
> 此方式最为简单，直接传入在线url参数即可。
>
> 支持平台：Android、IOS

```javascript
/**
* 打开在线文档
* @param {Object} fileUrl 文档url
*/
openOnlineFile(fileUrl) {
    sealOfficeOnlineModule.openFile(
        {
            url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
            title: 'Office文档在线预览', // 顶栏标题，默认为：APP名称
            topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#3394EC（科技蓝）
            waterMarkText: '你好，世界\n准备好了吗？时刻准备着' // 水印文本
            // installOfflineCore: true, // 是否离线安装内核
            // coreLocalPath: this.coreLocalPath, // 离线安装内核本地路径
        },
        res => {
            uni.showModal({
                content: '打开在线文档事件结果：' + JSON.stringify(res)
            });
        }
    );
},
```

### 4、WPS预览或编辑文档

使用接口：openFileWPS，参数参考《六、openFileWPS接口参数说明》

> WPS预览或编辑文档，是采用本机WPS客户端预览或编辑文档，支持pdf、txt、doc、xls、ppt等多种文件格式。
>
> 支持5种模式，包括文档编辑。前提，本地需要安装WPS客户端。
>
> 支持平台：Android、IOS（分享功能，第三方APP打开）

```javascript
/**
* WPS预览或编辑文档
* @param {String} fileUrl 文档url
* @param {String} openMode 打开模式
* openMode取值：
* Normal：正常模式，正常打开，WPS默认打开方式
* ReadOnly：只读模式，以只读的方式打开，WPS会隐藏编辑按钮
* EditMode：编辑模式，可对文档进行编辑
* ReadMode：阅读器模式，支持左右翻页，仅Word、TXT文档支持
* SaveOnly：另存模式(打开文件,另存,关闭)，仅Word、TXT文档支持
*/
openOnlineFileWPS(fileUrl, openMode) {
    sealOfficeOnlineModule.openFileWPS(
        {
            url: fileUrl,
            openMode
        },
        res => {
            uni.showModal({
                content: 'WPS打开文档事件结果：' + JSON.stringify(res)
            });
        }
    );
},
```

### 5、检查本机是否安装WPS客户端

使用接口：checkWps，无参数。

> 注意：返回结果格式：{ "hasWps": true }
>
> 支持平台：Android、IOS

```javascript
/**
* 检查WPS客户端是否已经安装
* 返回结果：{ "hasWps": true }
*/
checkWps() {
    const checkWps = sealOfficeOnlineModule.checkWps();
    console.log('checkWps', checkWps);
    uni.showModal({
        content: 'WPS是否安装：' + JSON.stringify(checkWps)
    });
},
```

### 6、QQ浏览服务预览文档

使用接口：openFileBS，参数参考《七、openFileBS接口参数说明》

> QQ浏览服务预览文档，是调用QQ浏览服务预览文档的方式，支持Excel在线编辑，PPT全屏浏览，查看最近打开文件，发送分享文档，采用其他应用打开等。
>
> 前提，需要本机安装QQ浏览器客户端。
>
> 支持平台：Android

```javascript
/**
* QQ浏览服务预览接口，支持Excel在线编辑，PPT全屏浏览，查看最近打开文件，发送分享文档，采用其他应用打开等
* 平台支持：仅Android，支持以下参数
* @param {Object} fileUrl 文档url
*/
openOnlineFileBS(fileUrl) {
    sealOfficeOnlineModule.openFileBS(
        {
            url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
            topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#3394EC（科技蓝）
            isDeleteFile: true // 退出是否删除缓存的文件，默认为true（删除缓存文件）
        },
        res => {
            uni.showModal({
                content: 'QQ浏览器打开文档事件结果：' + JSON.stringify(res)
            });
        }
    );
},
```

### 7、图片预览

使用接口：openFile

> 支持平台：Android、IOS

```javascript
/**
* 图片预览，支持jpg、jpeg、png、bmp、jpg、gif等多种常用图片格式
* 图片可以来源于列表或九宫格，传递给imageUrls数组
* @param {Object} fileUrl 图片url
* @param {Object} imageCurrentIndex 当前图片位置下标，从0开始
*/
openImage(fileUrl, imageCurrentIndex) {
    if (this.platform === 'android') {
        // Android
        sealOfficeOnlineModule.openFile({
            imageUrls: this.imageList,
            imageCurrentIndex, // 当前点击图片在imageUrls中的下标，从0开始，默认为0
            imageIndexType: 'number' // 图片底部指示器类型，默认为'dot'，可选：'number':数字；'dot':点
        });
    } else if (this.platform === 'ios') {
        // IOS
        sealOfficeOnlineModule.openFileImage(
            {
                url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
                title: 'IOS图片预览', // 顶栏标题，默认为：APP名称
                topBarBgColor: '#3394EC' // 顶栏背景颜色，默认为：#3394EC（科技蓝）
            },
            res => {
                uni.showModal({
                    content: 'IOS图片预览事件结果：' + JSON.stringify(res)
                });
            }
        );
    }
},
```

### 8、视频播放

使用接口：openFile

> 支持平台：Android、IOS

```javascript
/**
* 视频播放，支持市面上几乎所有的视频格式，包括mp4, flv, avi, 3gp, webm, ts, ogv, m3u8, asf, wmv, rm, rmvb, mov, mkv等18种视频格式
* 功能包括：全屏播放、锁屏、分享、画面比例调节、左边上下滑动调节亮度，右边上下滑动调节音量等
* 支持Android和IOS
* @param {Object} fileUrl 视频url
*/
openVideo(fileUrl) {
    sealOfficeOnlineModule.openFile(
        {
            videoUrl: fileUrl, // 视频在线url，此参数优先于图片预览和文档预览
            installOfflineCore: true, // 是否离线安装内核
            coreLocalPath: this.coreLocalPath // 离线安装内核本地路径
        },
        res => {
            uni.showModal({
                content: '播放视频事件结果：' + JSON.stringify(res)
            });
        }
    );
}
```

### 9、获取内核信息，用于调试

> 支持平台：Android

```javascript
/** 获取内核信息，用于调试 
* 返回结果格式
* {
*     'isCoreInited': false, // 内核是否加载
*     'coreVersion': 0, // 内核版本
*     'sdkVersion': 43967, // sdk版本
* }
*/
getX5CoreInfo() {
    const coreInfo = sealOfficeOnlineModule.getX5CoreInfo();
    console.log('coreInfo', coreInfo);
    uni.showModal({
        content: '插件内核信息：' + JSON.stringify(coreInfo)
    });
},
```



## 五、openFile接口参数说明

支持打开在线文档，本地文档

> 支持平台：Android、IOS

| 参数名             | 说明                                                         | 类型          | 是否必填 | 默认值            | 可选值                  |
| ------------------ | ------------------------------------------------------------ | ------------- | -------- | ----------------- | ----------------------- |
| url                | 支持如下三种地址方式：<br />（1）文件网络地址，如：http://113.62.127.199:8090/fileUpload/1.xlsx <br />（2）手机本地文件地址，如：/data/user/0/APP包名/files/1.xlsx 文件名，如：1.xlsx，<br />（3）访问默认目录文件，默认目录为：/data/user/0/APP包名，如：com.HBuilder.UniPlugin<br />**注意**：手机本地地址目录需要有权限访问<br/><span style="color:red">**IOS端只支持在线地址**</span> | string        | 是       |                   |                         |
| fileType           | 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型，<span style="color:red">**IOS端无此配置**</span> | string        | 否       |                   |                         |
| fileName           | 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType，<span style="color:red">**IOS端无此配置**</span> | string        | 否       |                   |                         |
| isDeleteFile       | 退出是否删除缓存的文件，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true              | false                   |
| initTitle          | 初始化插件动画标题，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 插件初始化        |                         |
| initBody           | 初始化插件动画内容，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 加载中...         |                         |
| docDownloadTitle   | 文档下载进度框标题，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 加载文档          |                         |
| docDownloadBody    | 文档下载进度框内容，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 请稍后...         |                         |
| installOfflineCore | 是否离线安装插件内核，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | false             | true                    |
| coreLocalPath      | 插件内核本地绝对路径，参考上面下载插件到本地用法，installOfflineCore=true时，必须配置，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | null              |                         |
| coreUrl            | 内核离线包url，coreLocalPath优先级更高，即，如果coreLocalPath不为空，coreUrl参数无效，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | null              |                         |
| waterMarkText      | 水印文本，默认以**\n**作为分隔符换行                         | string        | 否       | null              |                         |
| waterMarkTextSep   | 水印文本分隔符，<span style="color:red">**注意：IOS端只支持\n换行**</span> | string        | 否       | \n                |                         |
| waterMarkFontSize  | 水印字体大小，单位为sp<br>使用sp作为字体大小单位,会随着系统的字体大小改变 | int           | 否       | 13                |                         |
| waterMarkFontColor | 水印字体颜色                                                 | string        | 否       | #40F3F5F9         |                         |
| waterMarkDegree    | 水印旋转角度，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | -30（逆时针30度） |                         |
| isTopBar           | 是否显示顶栏，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true              | false                   |
| title              | 顶栏标题，isTopBar为true时有效                               | string        | 否       | APP名称           |                         |
| topBarAutoHide     | 顶栏是否自动隐藏，isTopBar=true时生效，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | false             | true                    |
| topBarHeight       | 顶栏自定义高度，isTopBar为true时有效，类型为正整数，<span style="color:red">**IOS端无此配置**</span> | int           | 否       | actionBarSize     |                         |
| topBarBgColor      | 顶栏背景颜色，isTopBar为true时有效                           | string        | 否       | #3394EC（科技蓝） |                         |
| topBarTextColor    | 顶栏文本颜色（isTopBar为true时有效）                         | string        | 否       | #FFFFFF（白色）   |                         |
| topBarTextLength   | 顶栏标题文字长度（isTopBar为true时有效），<span style="color:red">**IOS端无此配置**</span> | int           | 否       | 12                |                         |
| isBackArrow        | 是否显示返回按钮（isTopBar为true时有效），<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true              | false                   |
| videoUrl           | 视频在线url，此参数优先于图片预览和文档预览                  | string        | 是       |                   |                         |
| imageUrls          | 图片url数组，此参数优先于文档预览；长按图片底部弹出保存图片菜单，保存图片至相册，<span style="color:red">**IOS端无此配置**</span> | array<string> | 是       |                   |                         |
| imageCurrentIndex  | 当前点击图片在imageUrls中的下标，从0开始，<span style="color:red">**IOS端无此配置**</span> | int           | 否       | 0                 |                         |
| imageIndexType     | 图片底部指示器类型，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 'dot'             | 'number':数字；'dot':点 |



## 六、openFileWPS接口参数说明

本机WPS客户端预览或编辑文档

> 支持平台：Android、IOS（分享功能，第三方APP打开）

| 参数名           | 说明                                                         | 类型   | 是否必填 | 默认值    | 可选值 |
| ---------------- | ------------------------------------------------------------ | ------ | -------- | --------- | ------ |
| url              | 支持如下三种地址方式：<br />（1）文件网络地址，如：http://113.62.127.199:8090/fileUpload/1.xlsx <br />（2）手机本地文件地址，如：/data/user/0/APP包名/files/1.xlsx 文件名，如：1.xlsx，<br />（3）访问默认目录文件，默认目录为：/data/user/0/APP包名，如：com.HBuilder.UniPlugin<br />**注意**：手机本地地址目录需要有权限访问<br/><span style="color:red">**IOS端只支持在线地址**</span> | string | 是       |           |        |
| fileType         | 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型，<span style="color:red">**IOS端无此配置**</span> | string | 否       |           |        |
| fileName         | 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType，<span style="color:red">**IOS端无此配置**</span> | string | 否       |           |        |
| docDownloadTitle | 文档下载进度框标题，<span style="color:red">**IOS端无此配置**</span> | string | 否       | 加载文档  |        |
| docDownloadBody  | 文档下载进度框内容，<span style="color:red">**IOS端无此配置**</span> | string | 否       | 请稍后... |        |
| openMode         | 打开模式：<br>* Normal：正常模式，正常打开，WPS默认打开方式<br>* ReadOnly：只读模式，以只读的方式打开，WPS会隐藏编辑按钮<br>* EditMode：编辑模式，可对文档进行编辑<br>* ReadMode：阅读器模式，支持左右翻页，仅Word、TXT文档支持<br>* SaveOnly：另存模式(打开文件,另存,关闭)，仅Word、TXT文档支持 | string | 否       | Normal    | 见说明 |

另，openMode打开模式说明：

| 打开模式 | 说明                                                        |
| -------- | ----------------------------------------------------------- |
| Normal   | 正常模式，正常打开，WPS默认打开方式                         |
| ReadOnly | 只读模式，以只读的方式打开，WPS会隐藏编辑按钮               |
| EditMode | 编辑模式，可对文档进行编辑                                  |
| ReadMode | 阅读器模式，支持左右翻页，仅Word、TXT文档支持               |
| SaveOnly | （不常用）另存模式(打开文件,另存,关闭)，仅Word、TXT文档支持 |



## 七、openFileBS接口参数说明

QQ浏览服务打开在线文档

> 同openFile接口参数，支持：url，fileType，fileName，isDeleteFile，initTitle，initBody，docDownloadTitle，docDownloadBody，installOfflineCore，coreLocalPath，coreUrl，topBarBgColor
>
> 支持平台：Android

* 支持QQ浏览器在线编辑、全屏播放、阅读模式等
* 支持QQ浏览器打开46种文件格式文件
* 查看最近打开文件
* 发送分享文档
* 采用其他应用打开等



## 八、回调结果

### 1、回调结果格式：

```json
// openFile、OpenFileBS接口
{
    "code": 200,
    "msg": "文档预览成功",
    "result": {
        // 本地文件名
        "fileName": "test.docx",
        // 本地文件路径，前提，参数isDeleteFile=false，否则预览预览结束，本地文件被删除
        "filePath": "/storage/sdcard0/Android/data/com.seal.uniplugin/files/file/test.docx"
    }
}

// OpenFileWPS接口
{
    "code": 1021,
    "msg": "WPS动作事件",
    "result": {
        // 本地文件名
        "fileName": "test.docx",
        // 本地文件路径，前提，参数isDeleteFile=false，否则预览预览结束，本地文件被删除
        "filePath": "/storage/sdcard0/Android/data/com.seal.uniplugin/files/file/test.docx",
        // WPS动作类型：back，返回键；home，Home键；save，保存文件；close，关闭文件
        "actionType": "back",
        // 文件另存为本机路径
        "fileSavePath": "/storage/sdcard0/Android/Download/test2.docx"
    }
}
```

### 2、回调结果状态码说明：

| 状态码 | 说明                           |
| ------ | ------------------------------ |
| 200    | 文档预览成功                   |
|        |                                |
| 301    | 文档下载失败                   |
| 302    | 内核下载失败，自动重新下载     |
| 303    | 内核安装失败，自动重新安装     |
| 304    | 内核初始化失败，自动重新初始化 |
|        |                                |
| 1001   | 文档下载成功                   |
| 1002   | 开始初始化内核                 |
| 1003   | 开始离线安装内核               |
| 1004   | 开始在线安装内核               |
| 1005   | 内核下载成功                   |
| 1006   | 内核安装成功                   |
| 1007   | 内核初始化成功                 |
| 1008   | 缓存文档删除成功               |
|        |                                |
| 1021   | WPS动作事件                    |

### 3、WPS动作事件类型actionType说明

> 支持平台：Android

| 动作类型 | 说明     |
| -------- | -------- |
| back     | 返回键   |
| home     | Home键   |
| save     | 保存文件 |
| close    | 关闭文件 |



## 九、后续计划

- [x] 添加水印
- [x] 支持小窗口模式
- [ ] 支持右上角自定义菜单
- [x] 支持当前页面回调

## 十、Android预览效果

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

转载请注明：[我的技术分享](http://silianpan.cn) » [跨平台文件在线预览解决方案（四）-Android和IOS原生插件](http://silianpan.cn/archives/filepreviewuninative)
