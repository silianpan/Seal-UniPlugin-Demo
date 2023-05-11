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

## 〇、前言

2023年3月13日，腾讯发布了调整公告：[关于腾讯浏览服务内核SDK-内核文档能力调整公告](https://mp.weixin.qq.com/s/rmSa4Fs77MDdjFioRKwXPA)，公告里面明确说明了：**2023年4月13日零时起，内核文档能力正式下线**。插件2.x版本使用了腾讯X5云服务能力，**已经上线的项目需要及时升级插件到3.x版本哟**，3.x版本不再依赖腾讯X5以及任何第三方插件，完全可以在内网环境中使用，同时支持在线文档和设备本地文档。

### 原生Android项目离线集成，查看Demo工程及README文档： [Seal-Office-Android-Demo](https://github.com/silianpan/Seal-Office-Android-Demo)

<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询，也可以扫下面二维码进微信群。</span>

<img src="http://silianpan.cn/upload/2022/01/01/Seal-UniPlugin-WeiXin.jpeg" width="240" style="width:240px;" />

## 一、注意事项

* ##### 本插件不需要其他任何配置，也不需要依赖其他任何库，不要在manifest.json->App模块配置中勾选Android X5 Webview`

* ##### 不要同时使用其他同类文档预览插件，否则，可能会引起包冲突

* ##### url或参数中是否含有中文字符串，请使用encodeURIComponent**或**encodeURI进行转码，详细说明见下面“问题一”

## 二、问题解决

### 问题一：

> 插件安装成功，但是文档预览失败。

解决方案：检查文档url或参数中是否含有**中文字符串**，如果有，请使用**encodeURIComponent**或**encodeURI**进行转码。

如果参数有http://链接，使用**encodeURIComponent**；如果要对整个url进行转码，使用**encodeURI**

> encodeURIComponent和encodeURI区别
>
> 它们都是编码URL，唯一区别就是编码的字符范围，其中
>
> encodeURI方法***不会***对下列字符编码 **ASCII字母 数字 ~!@#$&\*()=:/,;?+'**
>
> encodeURIComponent方法***不会***对下列字符编码 **ASCII字母 数字 ~!\*()'**
>
> 所以encodeURIComponent比encodeURI编码的范围更大。



## 三、快速上手

Github克隆（[demo工程地址](https://github.com/silianpan/Seal-UniPlugin-Demo) ）或在右上角直接下载示例工程

安装开发工具：[HBuilderX](https://www.dcloud.io/hbuilderx.html)

安装依赖包环境：[NodeJS](https://nodejs.org/en)

### Step1. 下载demo工程，使用HBuilderX打开

* 代码根目录下执行

  ```bash
  npm install --registry https://registry.npm.taobao.org
  ```

<span style="color:red">注意：Vue版本要选择2，不要用3</span>

### Step2. 添加本文插件

插件名称：[Seal-OfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)

点击右上角`试用`或者`购买`，选择你的云打包插件

### Step3. 选择`manifest.json->App原生插件配置`中加载云端插件

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/eb31ccf0-15dc-11eb-8ff1-d5dcf8779628.png" width="600" style="width:600px" />

### Step4. 使用插件

* 在vue或nvue组件中，导入插件

```js
const sealOfficeOnlineModule = uni.requireNativePlugin("Seal-OfficeOnline")
```

* **openFile**方法（推荐）：支持Android和IOS，预览Office文件，支持如下格式：pdf、txt、doc、docx、xls、xlsx、ppt、pptx、epub等近50种类型文件，同时支持常见的音视频格式。
* **openFileWPS**方法（推荐）：采用本机WPS客户端预览或编辑文档，支持pdf、txt、doc、xls、ppt等多种文件格式。
* **checkWps**方法：检查本机WPS客户端是否已经安装。
* **openFileImage**方法：仅支持IOS，预览图片，支持如下格式：jpg、jpeg、png、bmp、jpg、gif等，参数同openFile方法

**<span style="color:red">接口使用方法，参考如下章节《四、使用方法》</span>**

### Step5. 调试

* 制作自定义调试基座：在开发工具中点击“运行到手机或模拟器-》制作自定义调试基座”
* 选择自定义调试基座：然后“运行到手机或模拟器-》基座运行选择-》自定义调试基座”
* 连接真机，运行到手机或模拟器-》运行到Android App基座，进行运行调试

## 四、使用方法

### 1、离线文档预览，非腾讯TBS，摆脱内核加载困扰，支持在线文档URL

使用接口：openFIle，参数参考章节《五、openFile接口参数说明》

> 离线文档预览，非腾讯TBS，无内核加载，摆脱腾讯X5内核加载失败的困扰。
>
> 同时支持在线文档URL或设备本地文档绝对路径。
>
> 两种使用方式，一，直接传递文档在线URL，二，先下载到设备本地，传递绝对路径，参考一下代码。
>
> 注意：添加参数`isDeleteFile: false`，否则退出预览，文件删除。
>
> 支持平台：Android、IOS

```javascript
/**
* 打开文档，非腾讯TBS，无内核加载，真正离线
* @param {Object} fileUrl 文档url
*/
openFile(fileUrl) {
  // 方式一：直接传递文件在线url
  sealOfficeOnlineModule.openFile(
    {
      url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
      // fileType: 'pdf',
      // fileName: 'example',
      title: 'Office文档在线预览', // 顶栏标题，默认为：APP名称
      topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#3394EC（科技蓝）
      waterMarkText: '你好，世界\n准备好了吗？时刻准备着', // 水印文本
      docRequestHeaders: {
        'Authorization': 'Token xxxxxxxx',
        'Other': 'other'
      }, // 文档下载请求头
      isDeleteFile: false,
      topBarAutoHide: true,
      isTopBar: true,
    },
    res => {
      this.printInfo('打开在线文档事件结果：', res);
    }
  );

  // 方式二：先下载文件保存到手机目录，再获取文件绝对路径
  // uni.showLoading({
  // 	title: '正在下载文件，请稍后~'
  // });
  // uni.downloadFile({
  // 	url: fileUrl,
  // 	success: res => {
  // 		if (res.statusCode === 200) {
  // 			// 直接传递本地文件地址
  // 			// 传递本地文件绝对路径，res.tempFilePath的前缀是_doc，而实际目录为doc，没有下划线_，所以要substr取子串
  // 			// const url = '/storage/sdcard0/Android/data/APP包名/apps/APPID/' + res.tempFilePath.substr(1)
  // 			// 可以通过以下方式获取文件绝对路径
  // 			uni.saveFile({
  // 				// 需要保存文件的临时路径
  // 				tempFilePath: res.tempFilePath,
  // 				success: resSave => {
  // 					uni.hideLoading();
  // 					const savedFilePath = resSave.savedFilePath;
  // 					// 转换为绝对路径
  // 					const url = plus.io.convertLocalFileSystemURL(savedFilePath);
  // 					console.log('tempFilePath', res.tempFilePath);
  // 					console.log('savedFilePath', savedFilePath);
  // 					console.log('url', url);
  // 					// 预览本地文件
  // 					sealOfficeOnlineModule.openFile(
  // 						{
  // 							url: url,
  // 							isDeleteFile: false
  // 						},
  // 						res => {
  // 							this.printInfo('打开本地文档事件结果：', res);
  // 						}
  // 					);
  // 				}
  // 			});
  // 		}
  // 	}
  // });
},
```

### 2、组件嵌入预览

组件名：Seal-OfficeOnline

> 组件嵌入预览，是采用nvue原生组件的方式，嵌入页面预览，用于页面的局部文档预览。
>
> 在线文档URL和离线设备文档均可预览，也支持自定义水印。
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



### 3、WPS预览或编辑文档

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

### 4、检查本机是否安装WPS客户端

##### 4.1、方法一（推荐）、使用原生接口：checkWps，无参数。

> 注意：返回结果格式：{ "hasWps": true }
>
> 支持平台：Android、IOS

<span style="color:red">注意：IOS配置应用访问白名单：</span>**manifest.json**—》App常用其他设置—》IOS设置—》应用访问白名单，

添加：**KingsoftOfficeApp,KingsoftOfficeAppEnterprise,WPSOfficeApi**

<img src="http://silianpan.cn/upload/2023/02/image-0c13acc0ef9e4a96a4bbd02f32361e30.png" alt="应用访问白名单" style="zoom: 50%;" />

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

##### 4.2、方法二、直接调用*plus.runtime.isApplicationExist*

参考链接：https://www.cnblogs.com/goloving/p/14384105.html

```js
// 判断第三方程序(WPS) 是否安装
function checkApp() {
    if (plus.runtime.isApplicationExist({pname:'cn.wps.moffice_eng', action:'KingsoftOfficeApp://'})) {
      console.log("WPS应用已安装");
    } else {
      console.log("WPS应用未安装");
    }
}
```



### 5、图片预览

使用接口：openFile（Android）、openFileImage（IOS）

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
            imageIndexType: 'number', // 图片底部指示器类型，默认为'dot'，可选：'number':数字；'dot':点
            isSaveImg: true, // 是否长按保存图片
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



### 6、音视频播放

使用接口：openFile

> 支持平台：Android、IOS

```javascript
/**
* 视频播放，支持市面上几乎所有的音视频格式，包括mp4, flv, avi, 3gp, webm, ts, ogv, m3u8, asf, wmv, rm, rmvb, mov, mkv等18种视频格式，以及mp3，wav，wma，flac等音频格式
* 功能包括：播放、暂停、重播、全屏播放、直播等
* 支持Android和IOS
* @param {String} fileUrl 音视频url
*/
openVideo(fileUrl) {
  sealOfficeOnlineModule.openFile(
    {
      videoUrl: fileUrl,
      isLive: true,
      title: '音视频播放标题',
      isTopBar: true,
      isBackArrow: false,
      topBarBgColor: '#F77234',
      topBarTextColor: '#FCF26B',
      topBarTextLength: 12
    },
    res => {
      this.printInfo('播放音视频事件结果：', res);
    }
  );
}
```



## 五、openFile接口参数说明

支持打开在线文档，本地文档

> 支持平台：Android、IOS

| 参数名             | 说明                                                         | 类型          | 是否必填 | 默认值            | 可选值                  |
| ------------------ | ------------------------------------------------------------ | ------------- | -------- | ----------------- | ----------------------- |
| url                | 支持如下三种地址方式：<br />（1）文件网络地址，如：http://113.62.127.199:8090/fileUpload/1.xlsx <br />（2）手机本地文件地址，如：/data/user/0/APP包名/files/1.xlsx 文件名，如：1.xlsx，<br />（3）访问默认目录文件，默认目录为：/data/user/0/APP包名，如：com.HBuilder.UniPlugin<br />**注意**：手机本地地址目录需要有权限访问<br/><span style="color:red">**IOS端只支持在线地址**</span> | string        | 是       |                   |                         |
| docRequestHeaders  | 文档下载请求头，如：<br />{ 'Authorization': 'Token xxxxxxxx', 'Other': 'other' }<br />，<span style="color:red">**IOS端无此配置**</span> | object        | 否       |                   |                         |
| fileType           | 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型 | string        | 否       |                   |                         |
| fileName           | 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType | string        | 否       |                   |                         |
| isDeleteFile       | 退出是否删除缓存的文件，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true              | false                   |
| docDownloadTitle   | 文档下载进度框标题，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 加载文档          |                         |
| docDownloadBody    | 文档下载进度框内容，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 请稍后...         |                         |
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
| videoUrl           | 音视频在线url，此参数优先于图片预览和文档预览                | string        | 是       |                   |                         |
| isLive             | 是否是视频直播，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | false             | true                    |
| imageUrls          | 图片url数组，此参数优先于文档预览；长按图片底部弹出保存图片菜单，保存图片至相册，<span style="color:red">**IOS端无此配置**</span> | array<string> | 是       |                   |                         |
| imageCurrentIndex  | 当前点击图片在imageUrls中的下标，从0开始，<span style="color:red">**IOS端无此配置**</span> | int           | 否       | 0                 |                         |
| imageIndexType     | 图片底部指示器类型，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 'dot'             | 'number':数字；'dot':点 |
| isSaveImg          | 是否长按保存图片，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | null              | true/false              |
| canScreenshot      | 是否可以截屏，<span style="color:blue">**3.0.2版本以上支持**</span>，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true（可以截屏）  | false（禁止截屏）       |



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



## 七、回调结果

### 1、回调结果格式

```json
// openFile接口
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

### 2、回调结果状态码说明

| 状态码 | 说明                        |
| ------ | --------------------------- |
| 200    | 文档预览成功                |
|        |                             |
| 301    | 文档下载失败                |
| 302    | 内核下载失败，自动重新下载  |
| 303    | 内核安装失败，自动重新安装  |
| 304    | 内核加载失败，可忽略该错误  |
| 305    | 内核下载失败，可忽略该错误  |
|        |                             |
| 1001   | 文档下载成功                |
| 1002   | 开始初始化内核              |
| 1003   | 开始离线安装内核            |
| 1004   | 开始在线安装内核            |
| 1005   | 内核下载成功                |
| 1006   | 内核安装成功                |
| 1007   | 内核初始化成功              |
| 1008   | 缓存文档删除成功            |
| 1009   | 程序员正在快马加鞭开发中... |
| 1010   | 页面返回                    |
|        |                             |
| 1021   | WPS动作事件                 |

### 3、WPS动作事件类型actionType说明

> 支持平台：Android

| 动作类型 | 说明     |
| -------- | -------- |
| back     | 返回键   |
| home     | Home键   |
| save     | 保存文件 |
| close    | 关闭文件 |



## 八、后续计划

- [x] 添加水印
- [x] 支持小窗口模式
- [ ] 支持右上角自定义菜单
- [x] 支持当前页面回调

## 九、Android预览效果

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
