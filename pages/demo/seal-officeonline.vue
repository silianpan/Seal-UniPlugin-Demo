<template>
	<view>
		<h2 class="title">openFile接口（Android和IOS）</h2>
		<u-cell-group title="Office文档预览"
			:title-style="{'font-size': '32rpx', 'font-weight': 'bold', 'color': '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in docList">
						<u-image @tap="openOnlineFile(item)" width="80%" height="180rpx"
							:src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
		<u-cell-group title="图片预览" :title-style="{'font-size': '32rpx', 'font-weight': 'bold', 'color': '#1890ff' }">
			<u-grid :col="3">
				<u-grid-item v-for="(item, index) in imageList">
					<u-image @tap="openImage(item, index)" width="100%" height="280rpx" :src="item" />
				</u-grid-item>
			</u-grid>
		</u-cell-group>
		<u-cell-group title="视频播放" :title-style="{'font-size': '32rpx', 'font-weight': 'bold', 'color': '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in videoList">
						<u-image @tap="openVideo(item)" width="80%" height="180rpx"
							:src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>

		<h2 class="title">openFileBS接口（Android）</h2>
		<u-cell-group title="文档/图片/视频预览"
			:title-style="{'font-size': '32rpx', 'font-weight': 'bold', 'color': '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in [...docList, ...imageList, ...videoList]">
						<u-image @tap="openOnlineFileBS(item)" width="80%" height="180rpx"
							:src="'/static/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
	</view>
</template>

<script>
	// get native module
	const testModule = uni.requireNativePlugin("Seal-OfficeOnline")
	const modal = uni.requireNativePlugin('modal')
	export default {
		data() {
			return {
				platform: '',
				docList: [
					'http://113.62.127.199:8090/fileUpload/1.pdf',
					'http://113.62.127.199:8090/fileUpload/1.txt',
					'http://113.62.127.199:8090/fileUpload/1.docx',
					'http://113.62.127.199:8090/fileUpload/1.xlsx',
					'http://113.62.127.199:8090/fileUpload/1.pptx'
				],
				imageList: [
					'http://113.62.127.199:8090/fileUpload/1.jpg',
					'http://113.62.127.199:8090/fileUpload/1.jpeg',
					'http://113.62.127.199:8090/fileUpload/1.png',
					'http://113.62.127.199:8090/fileUpload/1.bmp',
					'http://113.62.127.199:8090/fileUpload/1.gif'
				],
				videoList: [
					'http://113.62.127.199:8090/fileUpload/1.mp4',
					'http://113.62.127.199:8090/fileUpload/1.mkv',
					'http://113.62.127.199:8090/fileUpload/1.avi'
				]
			}
		},
		onLoad() {
			// 获取平台
			const {
				platform
			} = uni.getSystemInfoSync()
			this.platform = platform

			plus.globalEvent.addEventListener('SealEventCloseFile', function(e) {
				modal.toast({
					message: "SealEventCloseFile文件关闭事件：" + JSON.stringify(e),
					duration: 3
				})
			})
			// 获取内核信息
			this.getX5CoreInfo()
		},
		methods: {
			// 获取内核信息
			getX5CoreInfo() {
				const coreInfo = testModule.getX5CoreInfo()
				console.log('coreInfo', coreInfo)
			},
			openImage(fileUrl, imageCurrentIndex) {
				if (this.platform === 'android') {
					// Android
					testModule.openFile({
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
				testModule.openFile({
					videoUrl: fileUrl
				})
			},
			openOnlineFile(fileUrl) {
				testModule.openFile({
					url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
					isTopBar: true, // 是否显示顶栏，默认为：true（显示）
					title: 'Office文档在线预览', // 顶栏标题，默认为：APP名称
					// topBarHeight: 260, // 顶栏高度，默认为actionBarSize
					topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#177cb0（靛青）
					// topBarTextColor: '#cf1322', // 顶栏标题文字颜色，默认为：#FFFFFF（白色）
					// topBarTextLength: 5, // 顶栏标题文字长度，默认为：12
					isBackArrow: true, // 是否显示返回按钮，默认为：true（显示）
					// fileType: 'xlsx', // 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型
					// fileName: '1', // 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType
					// initTitle: '你好，世界', // 初始化插件动画标题，默认：'插件初始化'
					// initBody: '怎么了', // 初始化插件动画内容，默认：'加载中...'
					isDeleteFile: true, // 退出是否删除缓存的文件，默认为true（删除缓存文件）
				})
			},
			openOnlineFileBS(fileUrl) {
				testModule.openFileBS({
					url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
					topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#177cb0（靛青）
					// fileType: 'xlsx', // 可以指定文件类型，如：xlsx，在url参数无法判断文件类型时，可以指定文件类型
					// fileName: '1', // 指定文件名，如：file1，注意此处不带文件扩展名，如果同时指定fileName和fileType，那么最后的文件名通过这两个参数组合起来，即：fileName.fileType
					// initTitle: '你好，世界', // 初始化插件动画标题，默认：'插件初始化'
					// initBody: '怎么了', // 初始化插件动画内容，默认：'加载中...'
					isDeleteFile: true, // 退出是否删除缓存的文件，默认为true（删除缓存文件）
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
