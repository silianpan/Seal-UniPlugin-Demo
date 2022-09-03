<template>
	<view>
		<h2 class="title">openFile接口（Android和IOS）</h2>
		<u-cell-group title="Office在线文档预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in docList" :key="index">
						<u-image @tap="openOnlineFile(item)" width="80%" height="180rpx" :src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
		<u-cell-group title="Office本机文档预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in docList" :key="index">
						<u-image @tap="openOfflineFile(item)" width="80%" height="180rpx" :src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
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
					<u-grid-item v-for="(item, index) in [...docList, ...imageList, ...videoList]" :key="index">
						<u-image @tap="openOnlineFileBS(item)" width="80%" height="180rpx" :src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
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
				'http://silianpan.cn/upload/2022/01/01/1.pdf',
				'http://silianpan.cn/upload/2022/01/01/1.txt',
				'http://silianpan.cn/upload/2022/01/01/1.docx',
				'http://silianpan.cn/upload/2022/01/01/1.xlsx',
				'http://silianpan.cn/upload/2022/01/01/1.pptx',
				'http://silianpan.cn/upload/2022/01/01/1.epub'
			],
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

		plus.globalEvent.addEventListener('SealEventCloseFile', function(e) {
			modal.toast({
				message: 'SealEventCloseFile文件关闭事件：' + JSON.stringify(e),
				duration: 3
			});
		});
		// 获取内核信息
		this.getX5CoreInfo();
	},
	methods: {
		// 获取内核信息
		getX5CoreInfo() {
			const coreInfo = sealOfficeOnlineModule.getX5CoreInfo();
			console.log('coreInfo', coreInfo);
		},
		// 打开在线文件
		openOnlineFile(fileUrl) {
			sealOfficeOnlineModule.openFile({
				url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
				title: 'Office文档在线预览', // 顶栏标题，默认为：APP名称
				topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#177cb0（靛青）
				waterMarkText: '你好，世界\n准备好了吗？时刻准备着' // 水印文本
			});
		},
		// 打开本地文件
		openOfflineFile(fileUrl) {
			// 方式一：直接传递本机文件绝对路径
			// 注意：添加参数`isDeleteFile: false`，否则退出预览，文件删除
			// 比如：/storage/sdcard0/Android/data/com.seal.uniplugin/files/file/test.docx
			// sealOfficeOnlineModule.openFile({
			// 	url: '/storage/sdcard0/Android/data/com.seal.uniplugin/files/file/test.docx',
			// 	isDeleteFile: false
			// });
			
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
								sealOfficeOnlineModule.openFile({
									url: url,
									isDeleteFile: false
								});
							}
						});
					}
				}
			});
		},
		openOnlineFileBS(fileUrl) {
			sealOfficeOnlineModule.openFileBS({
				url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
				topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#177cb0（靛青）
				isDeleteFile: true // 退出是否删除缓存的文件，默认为true（删除缓存文件）
			});
		},
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
				this.openOnlineFile(fileUrl);
			}
		},
		openVideo(fileUrl) {
			sealOfficeOnlineModule.openFile({
				videoUrl: fileUrl
			});
		}
	}
};
</script>

<style>
.title {
	padding: 20rpx 30rpx 10rpx 30rpx;
}
</style>
