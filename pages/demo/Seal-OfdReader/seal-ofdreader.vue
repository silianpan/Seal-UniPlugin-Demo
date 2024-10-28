<template>
	<view>
		<h2 class="title">openFile接口（OFD文档离线预览）</h2>
		<view class="uni-input-wrapper" style="margin: 10px">
			<input class="uni-input" placeholder="请输入有效的在线文档地址或本地文档绝对路径" :value="inputFileUrl" @input="clearInput" />
			<text class="uni-icon" v-if="showClearIcon" @click="clearIcon">&#xe434;</text>
		</view>
		<u-button style="margin: 6px 10px 0 10px" type="primary" @click="handleInputOpenFile">打开输入框文档</u-button>
		<u-cell-group title="OFD文档预览" :title-style="{ 'font-size': '32rpx', 'font-weight': 'bold', color: '#1890ff' }">
			<u-grid :col="3">
				<u-grid :col="3">
					<u-grid-item v-for="(item, index) in docList" :key="index">
						<u-image @tap="showMenuDoc(item)" width="80%" height="180rpx" :src="'http://silianpan.cn/upload/2022/01/01/' + item.substring(item.lastIndexOf('.') + 1) + '.svg'" />
					</u-grid-item>
				</u-grid>
			</u-grid>
		</u-cell-group>
	</view>
</template>

<script>
// get native module
const sealOfdReaderModule = uni.requireNativePlugin('Seal-OfdReader');
const modal = uni.requireNativePlugin('modal');
export default {
	data() {
		return {
			inputFileUrl: '',
			showClearIcon: false,
			initPluginFirstSuccess: false,
			platform: '',
			docList: [
				'http://silianpan.cn/upload/2022/01/01/1.ofd',
				'http://silianpan.cn/upload/2022/01/01/2.pdf',
			]
		};
	},
	onLoad() {
		// 获取平台
		const { platform } = uni.getSystemInfoSync();
		this.platform = platform;
		
		if (platform === 'android') {
			// 初始化ofd阅读器引擎
			sealOfdReaderModule.initEngine((res) => {
				this.printInfo('初始化ofd阅读器引擎：', res);
			})
		}
	},
	methods: {
		// 打开输入框文档地址
		handleInputOpenFile() {
			if (this.inputFileUrl) {
				this.openFile(this.inputFileUrl)
			} else {
				uni.showToast({
					title: '请输入有效的在线文档地址或本地文档绝对路径'
				});
			}
		},
		// 清除输入框文档地址
		clearInput(event) {
			this.inputFileUrl = event.detail.value;
			if (event.detail.value.length > 0) {
				this.showClearIcon = true;
			} else {
				this.showClearIcon = false;
			}
		},
		clearIcon() {
			this.inputFileUrl = '';
			this.showClearIcon = false;
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
			// uni.showToast({
			// 	icon: null,
			// 	title: title + JSON.stringify(result),
			// })
			modal.toast({
				message: title + JSON.stringify(result),
				duration: 2,
			});
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
					'下载本地预览（非腾讯TBS，无内核加载，真正离线，自定义水印）',
					'禁止截屏预览（离线文档、组件嵌入均支持）',
				],
				success: ({ tapIndex }) => {
					switch (tapIndex) {
						case 0:
							this.openFile(fileUrl);
							break;
						case 1:
							uni.navigateTo({
								url: '/pages/demo/Seal-OfdReader/seal-ofdreader-component?url=' + fileUrl
							});
							break;
						case 2:
							this.openFile(fileUrl, {}, true);
							break;
						case 3:
							this.openFile(fileUrl, {
								// 禁止截屏
								canScreenshot: false,
							});
							break;
					}
				}
			});
		},
		/**
		 * 打开文档，非腾讯TBS，无内核加载，真正离线
		 * @param {Object} fileUrl 文档url
		 * @param {Object} otherOptions 其他选项
		 * @param {boolean} [downloadPreview=false] 是否下载预览
		 */
		openFile(fileUrl, otherOptions, downloadPreview = false) {
			if (downloadPreview) {
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
									const fileLocalPath = plus.io.convertLocalFileSystemURL(savedFilePath);
									console.log('tempFilePath', res.tempFilePath);
									console.log('savedFilePath', savedFilePath);
									console.log('fileLocalPath', fileLocalPath);
									// 预览本地文件
									sealOfdReaderModule.openFile(
										{
											url: fileLocalPath,
											isDeleteFile: true
										},
										res => {
											this.printInfo('打开本地文档事件结果：', res);
										}
									);
								}
							});
						}
					}
				});
			} else {
				// 方式一：直接传递文件在线url
				sealOfdReaderModule.openFile(
					{
						url: fileUrl, // 同时支持在线和本地文档，三种参数传递方式，具体查看文档说明
						// fileType: 'pdf',
						// fileName: 'example',
						title: 'OFD/PDF文档在线预览', // 顶栏标题，默认为：APP名称
						topBarBgColor: '#3394EC', // 顶栏背景颜色，默认为：#3394EC（科技蓝）
						// topBarBgColor: '#FFFFFF',
						// topBarTextColor: '#000000',
						isDark: false,
						waterMarkText: '你好，世界\n准备好了吗？时刻准备着', // 水印文本
						docRequestHeaders: {
							'Authorization': 'Token xxxxxxxx',
							'Other': 'other'
						}, // 文档下载请求头
						isDeleteFile: true,
						topBarAutoHide: true,
						isTopBar: true,
						// 顶部状态栏自定义菜单功能按钮
						menuItems: ['下载', '分享', '跳转第5页'],
						// 跳转页码
						targetPage: 5,
						...otherOptions,
					},
					res => {
						this.printInfo('打开在线文档事件结果：', res);
						if (res.code === 1012) {
							// 1012,导航栏菜单点击事件
							this.printInfo(res.result)
							if (res.result.menuItemId === 3) {
								this.printInfo('跳转第5页');
								sealOfdReaderModule.gotoPage(5);
							}
							
						}
					}
				);
			}
		},
	}
};
</script>

<style>
.title {
	padding: 20rpx 30rpx 10rpx 30rpx;
}
</style>
