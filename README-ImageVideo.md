## 引言

Seal-ImageVideo原生插件，实现了**图片预览**和**音视频播放**。

[Seal-ImageVideo插件下载使用地址](https://ext.dcloud.net.cn/plugin?id=5478)



## 〇、前言

2023年3月13日，腾讯发布了调整公告：[关于腾讯浏览服务内核SDK-内核文档能力调整公告](https://mp.weixin.qq.com/s/rmSa4Fs77MDdjFioRKwXPA)，公告里面明确说明了：**2023年4月13日零时起，内核文档能力正式下线**。插件2.x版本使用了腾讯X5云服务能力，**已经上线的项目需要及时升级插件到3.x版本哟**，3.x版本不再依赖腾讯X5以及任何第三方插件。

### 原生Android项目离线集成，查看Demo工程及README文档： [Seal-Office-Android-Demo](https://github.com/silianpan/Seal-Office-Android-Demo)

<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询，也可以扫下面二维码进微信群。</span>

<img src="http://silianpan.cn/upload/2022/01/01/Seal-UniPlugin-WeiXin.jpeg" width="240" style="width:240px;" />

## 一、注意事项

* ##### 本插件不需要其他任何配置，也不需要依赖其他任何库，不要在manifest.json->App模块配置中勾选Android X5 Webview`

* ##### 不要同时使用其他同类文档预览插件，否则，可能会引起包冲突



## 二、快速上手

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

插件名称：[Seal-ImageVideo](https://ext.dcloud.net.cn/plugin?id=5478)

点击右上角`试用`或者`购买`，选择你的云打包插件

### Step3. 选择`manifest.json->App原生插件配置`中加载云端插件

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/eb31ccf0-15dc-11eb-8ff1-d5dcf8779628.png" width="600" style="width:600px" />

### Step4. 使用插件

* 在vue或nvue组件中，导入插件

```js
const sealImageVideoModule = uni.requireNativePlugin('Seal-ImageVideo');
```

* **openFile**方法（推荐）：支持Android和IOS，预览Office文件，支持如下格式：pdf、txt、doc、docx、xls、xlsx、ppt、pptx、epub等近50种类型文件，同时支持常见的音视频格式。

**<span style="color:red">接口使用方法，参考如下章节《三、使用方法》</span>**

### Step5. 调试

* 制作自定义调试基座：在开发工具中点击“运行到手机或模拟器-》制作自定义调试基座”
* 选择自定义调试基座：然后“运行到手机或模拟器-》基座运行选择-》自定义调试基座”
* 连接真机，运行到手机或模拟器-》运行到Android App基座，进行运行调试

## 三、使用方法

### 1、图片预览

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
        sealImageVideoModule.openFile({
            imageUrls: this.imageList,
            imageCurrentIndex, // 当前点击图片在imageUrls中的下标，从0开始，默认为0
            imageIndexType: 'number', // 图片底部指示器类型，默认为'dot'，可选：'number':数字；'dot':点
            isSaveImg: true, // 是否长按保存图片
        });
    } else if (this.platform === 'ios') {
        // IOS
        sealImageVideoModule.openFile(
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



### 2、音视频播放

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
  sealImageVideoModule.openFile(
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



## 四、openFile接口参数说明

支持打开在线文档，本地文档

> 支持平台：Android、IOS

| 参数名            | 说明                                                         | 类型          | 是否必填 | 默认值            | 可选值                  |
| ----------------- | ------------------------------------------------------------ | ------------- | -------- | ----------------- | ----------------------- |
| isTopBar          | 是否显示顶栏，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true              | false                   |
| title             | 顶栏标题，isTopBar为true时有效                               | string        | 否       | APP名称           |                         |
| topBarHeight      | 顶栏自定义高度，isTopBar为true时有效，类型为正整数，<span style="color:red">**IOS端无此配置**</span> | int           | 否       | actionBarSize     |                         |
| topBarBgColor     | 顶栏背景颜色，isTopBar为true时有效                           | string        | 否       | #3394EC（科技蓝） |                         |
| topBarTextColor   | 顶栏文本颜色（isTopBar为true时有效）                         | string        | 否       | #FFFFFF（白色）   |                         |
| topBarTextLength  | 顶栏标题文字长度（isTopBar为true时有效），<span style="color:red">**IOS端无此配置**</span> | int           | 否       | 12                |                         |
| isBackArrow       | 是否显示返回按钮（isTopBar为true时有效），<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true              | false                   |
| videoUrl          | 视频在线url，此参数优先于图片预览和文档预览                  | string        | 是       |                   |                         |
| isLive            | 是否是视频直播，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | false             | true                    |
| imageUrls         | 图片url数组，此参数优先于文档预览；长按图片底部弹出保存图片菜单，保存图片至相册，<span style="color:red">**IOS端无此配置**</span> | array<string> | 是       |                   |                         |
| imageCurrentIndex | 当前点击图片在imageUrls中的下标，从0开始，<span style="color:red">**IOS端无此配置**</span> | int           | 否       | 0                 |                         |
| imageIndexType    | 图片底部指示器类型，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 'dot'             | 'number':数字；'dot':点 |
| isSaveImg         | 是否长按保存图片，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | null              | true/false              |
| canScreenshot     | 是否可以截屏，<span style="color:blue">**3.0.2版本以上支持**</span>，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true（可以截屏）  | false（禁止截屏）       |

## 五、回调结果

### 1、回调结果格式

```json
// openFile接口
{
    "code": 1010,
    "msg": "页面返回"
}
```

### 2、回调结果状态码说明

| 状态码 | 说明     |
| ------ | -------- |
| 1010   | 页面返回 |

