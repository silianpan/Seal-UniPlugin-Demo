<template>
	<view>
		<u-cell-group title="图片预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid-item v-for="(item, index) in imageList" :key="index"><u-image @tap="openImage(item, index)" width="100%" height="280rpx" :src="item" /></u-grid-item>
			</u-grid>
		</u-cell-group>

		<u-cell-group title="视频播放" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
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
const sealImageVideoModule = uni.requireNativePlugin('Seal-ImageVideo')
export default {
	data() {
		return {
			platform: '',
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
				'http://silianpan.cn/upload/2022/01/01/1.avi'
			]
		}
	},
	onLoad() {
		// 获取平台
		const { platform } = uni.getSystemInfoSync()
		this.platform = platform
		// 获取内核信息
		this.getX5CoreInfo();
	},
	methods: {
		// 获取内核信息
		getX5CoreInfo() {
			const coreInfo = sealImageVideoModule.getX5CoreInfo();
			console.log('coreInfo', coreInfo);
		},
		openImage(fileUrl, imageCurrentIndex) {
			if (this.platform === 'android') {
				// Android
				sealImageVideoModule.openFile({
					imageUrls: this.imageList,
					imageCurrentIndex, // 当前点击图片在imageUrls中的下标，从0开始，默认为0
					imageIndexType: 'number' // 图片底部指示器类型，默认为'dot'，可选：'number':数字；'dot':点
				})
			} else if (this.platform === 'ios') {
				// IOS
				this.openOnlineFile(fileUrl)
			}
		},
		openVideo(fileUrl) {
			sealImageVideoModule.openFile({
				videoUrl: fileUrl
			})
		},
		openOnlineFile(fileUrl) {
			sealImageVideoModule.openFile({
				url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
				title: '视频在线播放', // 顶栏标题，默认为：APP名称
				topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#177cb0（靛青）
			})
		}
	}
}
</script>

<style>
.title {
	padding: 20rpx 30rpx 10rpx 30rpx;
}
</style>
