## 引言

Seal-ImageVideo原生插件，实现了**图片预览**和**视频播放**。

[Seal-ImageVideo插件下载使用地址](https://ext.dcloud.net.cn/plugin?id=5478)

<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询。</span>

<div class="half">

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-3ace2e69-dfbc-47f2-906f-58c695d12020/5757c1c9-20b6-46dc-9540-78621476601e.jpg" height="560" style="height:560px" />

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-3ace2e69-dfbc-47f2-906f-58c695d12020/fa8e89f6-6cf0-4a5a-aac2-87785ffd5899.jpg" height="560" style="height:560px" />

</div>

## 注意事项

* ### `manifest.json->App模块配置不要勾选Android X5 Webview`

* ### 不要同时使用其他任何预览插件，否则，会引起打包冲突

* ### 如遇预览失败，（1）把APP卸载，重新打基座运行，第一次运行会自动安装X5插件；（2）或者换手机（华为或小米）进行测试



<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询。</span>



## 快速上手

[demo工程地址](https://github.com/silianpan/Seal-UniPlugin-Demo) 或在右上角直接下载示例工程

开发工具：[HBuilderX](https://www.dcloud.io/hbuilderx.html)

### Step1. 下载demo工程，使用HBuilderX打开

### Step2. 下载本文插件

插件名称：[Seal-ImageVideo](https://ext.dcloud.net.cn/plugin?id=5478)

点击右上角`试用`或者`购买`，选择你的云打包插件

### Step3. 选择`manifest.json->App原生插件配置`中加载云端插件

<img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-aliyun-lau3cirf3bhq53ac04/eb31ccf0-15dc-11eb-8ff1-d5dcf8779628.png" width="600" style="width:600px" />

### Step4. 使用插件

* 在vue或nvue组件中，导入插件

```js
const sealImageVideoModule = uni.requireNativePlugin("Seal-ImageVideo")
```

* **openFile**方法（推荐）：支持Android和IOS，在线预览图片，播放视频，支持jpg、jpeg、png、bmp、gif、mp4、mkv、avi等多种格式。
* **getX5CoreInfo**方法（调试）：获取内核安装信息，用于调试

#### （1）图片预览

```javascript
// 图片预览，支持jpg、jpeg、png、bmp、jpg、gif等多种常用图片格式
// 图片可以来源于列表或九宫格，传递给imageUrls数组
const url = 'http://silianpan.cn/upload/2022/01/01/'
sealImageVideoModule.openFile({
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
```

#### （2）视频播放

```js
// 视频播放，支持市面上几乎所有的视频格式，包括mp4, flv, avi, 3gp, webm, ts, ogv, m3u8, asf, wmv, rm, rmvb, mov, mkv等18种视频格式
// 功能包括：全屏播放、锁屏、分享、画面比例调节、左边上下滑动调节亮度，右边上下滑动调节音量等
// 支持Android和IOS
sealImageVideoModule.openFile({
    videoUrl: 'http://silianpan.cn/upload/2022/01/01/1.mp4', // 视频在线url，此参数优先于图片预览和文档预览
})
```

#### （3）获取内核信息，用于调试

```javascript
// 获取内核信息，用于调试
const coreInfo = sealOfficeOnlineModule.getX5CoreInfo()
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

| 参数名             | 说明                                                         | 类型          | 是否必填 | 默认值 | 可选值                  |
| ------------------ | ------------------------------------------------------------ | ------------- | -------- | ------ | ----------------------- |
| videoUrl           | 视频在线url，此参数优先于图片预览和文档预览                  | string        | 是       |        |                         |
| imageUrls          | 图片url数组，此参数优先于文档预览；长按图片底部弹出保存图片菜单，保存图片至相册，<span style="color:red">**IOS端无此配置**</span> | array<string> | 是       |        |                         |
| installOfflineCore | 是否离线安装插件内核，<span style="color:red">**IOS端无此配置**</span> | bool          | 否       | true   | false                   |
| imageCurrentIndex  | 当前点击图片在imageUrls中的下标，从0开始，<span style="color:red">**IOS端无此配置**</span> | int           | 否       | 0      |                         |
| imageIndexType     | 图片底部指示器类型，<span style="color:red">**IOS端无此配置**</span> | string        | 否       | 'dot'  | 'number':数字；'dot':点 |



## 问题解决

### 问题一：

> 问题描述：真机测试有4部手机测试， 小米MAX3、坚果pro2 、华为P20 这个三个手机测试预览都能成功，小米redmi K30 预览 就不成功，
> "canLoadVideo": false, "canLoadX5": false, "coreVersion": 0, "sdkVersion": 43967, "canLoadX5FirstTimeThirdApp": false, "isCoreInited": true
> X5内核没有加载成功，反复卸载APP和重新打包基座好几次了，X5内核都是没有加载成功，这个问题该如何解决

后面想了一下可能是CPU支持类型的问题，

**解决方案：manifest.json -APP常用其他设置 -支持CPU类型 armeabi-v7a arm64-v8a 两个都勾选的**，

重新打一下自定义基座 问题就解决了
