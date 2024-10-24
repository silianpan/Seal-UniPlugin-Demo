<template>
	<view>
		<h2 class="title">openFile接口（Android非腾讯TBS，无内核加载，真正离线和IOS）</h2>
		<u-cell-group title="图片预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid-item v-for="(item, index) in imageList" :key="index"><u-image @tap="openImage(item, index)" width="100%" height="280rpx" :src="item" /></u-grid-item>
			</u-grid>
		</u-cell-group>
		<u-cell-group title="音视频播放" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in videoList" :key="index">
						<u-image @tap="openVideo(item)" width="80%" height="180rpx" :src="'http://silianpan.cn/upload/2022/01/01/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
	</view>
</template>

<script>
// get native module
const sealImageVideoModule = uni.requireNativePlugin('Seal-ImageVideo');
const modal = uni.requireNativePlugin('modal');
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
	},
	methods: {
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
		 * 图片预览
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
					isSaveImg: true,
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
	}
};
</script>

<style>
.title {
	padding: 20rpx 30rpx 10rpx 30rpx;
}
</style>
