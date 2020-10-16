<template>
	<view class="button-sp-area">
		<button type="primary" plain="true" @click="showRichAlert()">点击显示弹窗</button>
	</view>
</template>

<script>
	const modal = uni.requireNativePlugin('modal');
	const dcRichAlert = uni.requireNativePlugin('DCloud-RichAlert');
	export default {
		data() {
			return {
				title: ''
			}
		},
		onLoad() {

		},
		methods: {
			showRichAlert() {
				dcRichAlert.show({
					position: 'bottom',
					title: "提示信息",
					titleColor: '#FF0000',
					content: "<a href='https://uniapp.dcloud.io/' value='Hello uni-app'>uni-app</a> 是一个使用 Vue.js 开发跨平台应用的前端框架!\n免费的\n免费的\n免费的\n重要的事情说三遍",
					contentAlign: 'left',
					checkBox: {
						title: '不再提示',
						isSelected: true
					},
					buttons: [{
							title: '取消'
						},
						{
							title: '否'
						},
						{
							title: '确认',
							titleColor: '#3F51B5'
						}
					]
				}, result => {
					const msg = JSON.stringify(result);
					modal.toast({
						message: msg,
						duration: 1.5
					});
					switch (result.type) {
						case 'button':
							console.log("callback---button--" + result.index);
							break;
						case 'checkBox':
							console.log("callback---checkBox--" + result.isSelected);
							break;
						case 'a':
							console.log("callback---a--" + JSON.stringify(result));
							break;
						case 'backCancel':
							console.log("callback---backCancel--");
							break;
					}
				});
			},

			// 			nvueclick() {
			// 				uni.navigateTo({
			// 					url: '/pages/nvue/index2'
			// 				});
			// 			}
		}
	}
</script>

<style>
	button {
		margin-top: 30upx;
		margin-bottom: 30upx;
	}

	.button-sp-area {
		margin: 0 auto;
		width: 60%;
	}

	.content {
		text-align: center;
		height: 400upx;
	}

	.wrapper {
		flex-direction: column;
		justify-content: center;
	}

	.button {
		width: 200px;
		margin-top: 30px;
		margin-left: 20px;
		padding-top: 20px;
		padding-bottom: 20px;
		border-width: 2px;
		border-style: solid;
		border-color: #458B00;
		background-color: #458B00;
	}

	.text {
		font-size: 30px;
		color: #666666;
		text-align: center;
	}
</style>
