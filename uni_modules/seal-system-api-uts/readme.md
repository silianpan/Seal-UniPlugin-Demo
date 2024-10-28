## 〇、推荐插件 

### 作者其他插件，欢迎使用~

#### 1、原生Android项目离线集成，Demo工程及README文档地址： [Seal-Office-Android-Demo](https://github.com/silianpan/Seal-Office-Android-Demo)

#### 2、跨平台OFD、PDF文档预览原生插件，插件地址：[Seal-OfdReader]()

#### 3、跨平台Office文档预览原生插件【非X5离线、组件嵌入、水印、WPS预览编辑】，插件地址：[Seal-OfficeOnline](https://ext.dcloud.net.cn/plugin?id=3226)

#### 4、Android和IOS图片预览，音视频播放原生插件【非腾讯X5，无内核加载】，插件地址：[Seal-ImageVideo](https://ext.dcloud.net.cn/plugin?id=5478)

#### 5、跨平台Android和IOS百度OCR文字识别、证卡识别、票据识别原生插件，插件地址：[Seal-OCR](https://ext.dcloud.net.cn/plugin?id=7725)

#### 6、跨平台Android和IOS百度语音在线识别原生插件，插件地址：[Seal-VoiceASR](https://ext.dcloud.net.cn/plugin?id=8950)

<span style="color:red">各位同学，对于插件使用还有疑问的，可以加QQ群（170683293）咨询，也可以扫下面二维码添加WX或者添加QQ（2480621579）。</span>

<img src="http://silianpan.cn/upload/2022/01/01/Seal-UniPlugin-WeiXin-Me.jpg" width="240" style="width:240px;" />



## 一、快速上手

Github克隆（[demo工程地址](https://github.com/silianpan/Seal-UniPlugin-UTS-Demo) ）或在右上角直接下载示例工程

安装开发工具：[HBuilderX](https://www.dcloud.io/hbuilderx.html)

### Step1. 下载demo工程，使用HBuilderX打开

### Step2. 添加本文插件

插件名称：seal-system-api-uts

点击右上角`试用`或者`购买`，选择你的云打包插件

### Step3. 选择`manifest.json->App原生插件配置`中加载云端插件

### Step4. 使用插件，参考如下：二、使用方法

### Step5. 调试

* 制作自定义调试基座：在开发工具中点击“运行到手机或模拟器-》制作自定义调试基座”
* 选择自定义调试基座：然后“运行到手机或模拟器-》基座运行选择-》自定义调试基座”
* 连接真机，运行到手机或模拟器-》运行到Android App基座，进行运行调试

## 二、使用方法

index.nvue

```vue
<template>
	<view>
		<page-head title="系统接口"></page-head>
			<view class="uni-btn-v uni-common-mt">
        <button type="primary"  @tap="testGetRingerMode">获取静音模式状态</button>
        <button type="primary"  @tap="testGetDndStatus">获取免打扰状态</button>
        <button type="primary"  @tap="testGetStreamVolume">获取音量</button>
			</view>
	</view>
</template>
<script>
  import { getRingerMode, getDndStatus, getStreamVolume, InfoOptions } from "../../uni_modules/seal-system-api-uts";
	
	export default {
		methods: {
      printInfo(obj: object) {
        console.log('obj', obj)
        uni.showToast({
            icon: 'none',
            title: JSON.stringify(obj),
            duration: 3000
        })
      },
      // 获取静音模式状态
      testGetRingerMode() {
        getRingerMode({
          success: (res: object) => {
            this.printInfo(res)
          },
          fail: (err: object) => {
            this.printInfo(err)
          }
        } as InfoOptions)
      },
      // 获取免打扰状态
      testGetDndStatus() {
        getDndStatus({
          success: (res: object) => {
            this.printInfo(res)
          },
          fail: (err: object) => {
            this.printInfo(err)
          }
        } as InfoOptions)
      },
      // 获取音量
      testGetStreamVolume() {
        getStreamVolume({
          success: (res: object) => {
            this.printInfo(res)
          },
          fail: (err: object) => {
            this.printInfo(err)
          }
        } as InfoOptions)
      }
		}
	}
</script>
```

index.vue

```vue
<template>
	<view>
		<page-head title="系统接口"></page-head>
			<view class="uni-btn-v uni-common-mt">
        <button type="primary"  @tap="testGetRingerMode">获取静音模式状态</button>
        <button type="primary"  @tap="testGetDndStatus">获取免打扰状态</button>
        <button type="primary"  @tap="testGetStreamVolume">获取音量</button>
			</view>
	</view>
</template>
<script>
  import { getRingerMode, getDndStatus, getStreamVolume } from "../../uni_modules/seal-system-api-uts";
	
	export default {
		methods: {
      printInfo(obj) {
        console.log('obj', obj)
        uni.showToast({
            icon: 'none',
            title: JSON.stringify(obj),
            duration: 3000
        })
      },
      // 获取静音模式状态
      testGetRingerMode() {
        getRingerMode({
          success: (res) => {
            this.printInfo(res)
          },
          fail: (err) => {
            this.printInfo(err)
          }
        })
      },
      // 获取免打扰状态
      testGetDndStatus() {
        getDndStatus({
          success: (res) => {
            this.printInfo(res)
          },
          fail: (err) => {
            this.printInfo(err)
          }
        })
      },
      // 获取音量
      testGetStreamVolume() {
        getStreamVolume({
          success: (res) => {
            this.printInfo(res)
          },
          fail: (err) => {
            this.printInfo(err)
          }
        })
      }
		}
	}
</script>
```

