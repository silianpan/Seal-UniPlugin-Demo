<template>
	<view>
		<h2 class="title">openFile接口（Android非腾讯TBS，无内核加载，真正离线和IOS）</h2>
		<u-button style="margin: 6px 10px 0 10px" type="primary" @click="handlePlusCheckWPS">plus检查WPS是否安装</u-button>
		<u-cell-group title="Office文档预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in docList" :key="index">
						<u-image @tap="showMenuDoc(item)" width="80%" height="180rpx" :src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
		<u-cell-group title="图片预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid-item v-for="(item, index) in imageList" :key="index"><u-image @tap="openImage(item, index)" width="100%" height="280rpx" :src="item" /></u-grid-item>
			</u-grid>
		</u-cell-group>
		<u-cell-group title="音视频播放" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in videoList" :key="index">
						<u-image @tap="openVideo(item)" width="80%" height="180rpx" :src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
	</view>
</template>

<script>
// get native module
const sealOfficeOnlineModule = uni.requireNativePlugin('Seal-OfficeOnline');
const modal = uni.requireNativePlugin('modal');
export default {
	data() {
		return {
			platform: '',
			docList: [
				'http://silianpan.cn/upload/2022/01/01/2.pdf',
				'http://silianpan.cn/upload/2022/01/01/1.txt',
				'http://silianpan.cn/upload/2022/01/01/1.docx',
				'http://silianpan.cn/upload/2022/01/01/1.xlsx',
				'http://silianpan.cn/upload/2022/01/01/1.pptx',
				'http://silianpan.cn/upload/2022/01/01/1.csv'
			],
			imageList: [
				'http://silianpan.cn/upload/2022/01/01/1.jpg',
				'http://silianpan.cn/upload/2022/01/01/1.jpeg',
				'http://silianpan.cn/upload/2022/01/01/1.png',
				'http://silianpan.cn/upload/2022/01/01/1.bmp',
				'http://silianpan.cn/upload/2022/01/01/1.gif'
			],
			videoList: [
				'http://silianpan.cn/upload/2022/01/01/1.mp4',
				'http://silianpan.cn/upload/2022/01/01/1.mkv',
				'http://silianpan.cn/upload/2022/01/01/1.avi',
				'http://silianpan.cn/upload/2022/01/01/1.mp3',
				'http://silianpan.cn/upload/2022/01/01/1.wav',
				'http://silianpan.cn/upload/2022/01/01/1.flac'
			]
		};
	},
	onLoad() {
		// 获取平台
		const { platform } = uni.getSystemInfoSync();
		this.platform = platform;

		if (this.platform === 'android') {}
		// 检查WPS客户端是否已经安装
		this.checkWps();
	},
	methods: {
		// 判断第三方程序(WPS) 是否安装
		handlePlusCheckWPS() {
			const isExistApp = plus.runtime.isApplicationExist({pname:'cn.wps.moffice_eng', action:'KingsoftOfficeApp://'});
			if (isExistApp) {
				console.log("WPS应用已安装");
				uni.showModal({
					content: 'WPS应用已安装'
				});
			} else {
				console.log("WPS应用未安装");
				uni.showModal({
					content: 'WPS应用未安装'
				});
			}
		},
		/**
		 * 打印结果信息
		 * @param {Object} title 标题
		 * @param {Object} result 结果
		 */
		printInfo(title, result) {
			console.log(title, result);
			// uni.showModal({
			// 	content: title + JSON.stringify(result)
			// });
		},
		/**
		 * 检查WPS客户端是否已经安装
		 */
		checkWps() {
			const checkWps = sealOfficeOnlineModule.checkWps();
			this.printInfo('WPS是否安装：', checkWps);
		},
		/**
		 * 打开文档预览选项框
		 * @param {String} fileUrl 文档url
		 */
		showMenuDoc(fileUrl) {
			uni.showActionSheet({
				itemList: [
					'离线文档预览（非腾讯TBS，无内核加载，真正离线，自定义水印、顶栏）',
					'组件嵌入预览（非腾讯TBS，无内核加载，真正离线，自定义水印）',
					'禁止截屏预览（离线文档、组件嵌入均支持）',
					'WPS打开文档（正常模式，需安装WPS客户端）',
					'WPS打开文档（只读模式，需安装WPS客户端）',
					'WPS打开文档（编辑模式，需安装WPS客户端）',
					'WPS打开文档（阅读器模式，需安装WPS客户端）',
					'WPS打开文档（另存模式，需安装WPS客户端）'
				],
				success: ({ tapIndex }) => {
					switch (tapIndex) {
						case 0:
							this.openFile(fileUrl);
							break;
						case 1:
							uni.navigateTo({
								url: '/pages/demo/Seal-OfficeOnline/seal-officeonline-component?url=' + fileUrl
							});
							break;
						case 2:
							this.openFile(fileUrl, {
								// 禁止截屏
								canScreenshot: false,
							});
							break;
						case 3:
							this.openOnlineFileWPS(fileUrl, 'Normal');
							break;
						case 4:
							this.openOnlineFileWPS(fileUrl, 'ReadOnly');
							break;
						case 5:
							this.openOnlineFileWPS(fileUrl, 'EditMode');
							break;
						case 6:
							this.openOnlineFileWPS(fileUrl, 'ReadMode');
							break;
						case 7:
							this.openOnlineFileWPS(fileUrl, 'SaveOnly');
							break;
					}
				}
			});
		},
		/**
		 * 打开文档，非腾讯TBS，无内核加载，真正离线
		 * @param {Object} fileUrl 文档url
		 */
		openFile(fileUrl, otherOptions) {
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
					...otherOptions,
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
					this.printInfo('WPS打开文档事件结果：', res);
				}
			);
		},
		/**
		 * 图片预览
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
					isSaveImg: true,
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
						this.printInfo('IOS图片预览事件结果：', res);
					}
				);
			}
		},
		/**
		 * 音视频播放
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
	}
};
</script>

<style>
.title {
	padding: 20rpx 30rpx 10rpx 30rpx;
}
</style>
