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
		
		<h2 class="title">openFileBS接口（Android）</h2>
		<u-cell-group title="文档/图片/视频预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in [...imageList, ...videoList]" :key="index">
						<u-image @tap="openOnlineFileBS(item)" width="80%" height="180rpx" :src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
	</view>
</template>

<script>
// get native module
const sealImageVideoModule = uni.requireNativePlugin('Seal-ImageVideo');
export default {
	data() {
		return {
			// 本地内核地址
			coreLocalPath: '',
			platform: '',
			imageList: [
				'http://silianpan.cn/upload/2022/01/01/1.jpg',
				'http://silianpan.cn/upload/2022/01/01/1.jpeg',
				'http://silianpan.cn/upload/2022/01/01/1.png',
				'http://silianpan.cn/upload/2022/01/01/1.bmp',
				'http://silianpan.cn/upload/2022/01/01/1.gif'
			],
			videoList: ['http://silianpan.cn/upload/2022/01/01/1.mp4', 'http://silianpan.cn/upload/2022/01/01/1.mkv', 'http://silianpan.cn/upload/2022/01/01/1.avi']
		};
	},
	onLoad() {
		// 获取平台
		const { platform } = uni.getSystemInfoSync();
		this.platform = platform;

		if (this.platform === 'android') {
			// 下载内核
			// this.downloadCoreToLocal();
			// 获取内核信息
			this.getX5CoreInfo();
		}
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
		 * 获取内核信息
		 */
		getX5CoreInfo() {
			const coreInfo = sealImageVideoModule.getX5CoreInfo();
			this.printInfo('插件内核信息：', coreInfo);
		},
		/**
		 * 应用刚进入或页面刚进入，初始化下载内核到本机，获取本地路径
		 */
		downloadCoreToLocal() {
			uni.showLoading({
				title: '正在下载安装内核，请稍后~'
			});
			// const coreUrl = 'https://tbs.imtt.qq.com/release/x5/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release.tbs'
			const coreUrl = 'http://silianpan.cn/upload/2022/09/tbs_core_045738_20210925205342_nolog_fs_obfs_armeabi_release-c5d6cd8bba8843b780e5943f9806c028.tbs';
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
		 * 图片预览，分Android和IOS平台
		 * @param {String} fileUrl 图片url
		 * @param {Number} imageCurrentIndex 图片下标，从0开始
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
						this.printInfo('IOS图片预览事件结果：', res);
					}
				);
			}
		},
		/**
		 * 视频播放接口
		 * @param {String} fileUrl 视频url
		 */
		openVideo(fileUrl) {
			sealImageVideoModule.openFile(
				{
					videoUrl: fileUrl,
					installOfflineCore: true, // 是否离线安装内核
					coreLocalPath: this.coreLocalPath // 离线安装内核本地路径
				},
				res => {
					this.printInfo('播放视频事件结果：', res);
				}
			);
		},
		/**
		 * QQ浏览器预览接口
		 * @param {Object} fileUrl 文档url
		 */
		openOnlineFileBS(fileUrl) {
			sealImageVideoModule.openFileBS(
				{
					url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
					topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#3394EC（科技蓝）
					isDeleteFile: true // 退出是否删除缓存的文件，默认为true（删除缓存文件）
				},
				res => {
					this.printInfo('QQ浏览器打开文档事件结果：', res);
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
