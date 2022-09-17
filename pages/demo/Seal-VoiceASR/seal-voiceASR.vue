<template>
	<view class="voice-container">
		<textarea style="border:1px solid #ccc;width: 100%;" placeholder="识别结果" v-model="recogText"></textarea>
		<u-button plain type="primary" @click="handleOnlineRecogStart">{{ recogOnlineBtn }}</u-button>
		<u-button plain type="primary" @click="handleOnlineRecogEnd">结束在线识别</u-button>
	</view>
</template>

<script>
const sealVoiceASRModule = uni.requireNativePlugin('Seal-VoiceASR');
const modal = uni.requireNativePlugin('modal');
export default {
	data() {
		return {
			recogOnlineBtn: '开始在线识别',
			// 识别文本
			recogText: ''
		};
	},
	methods: {
		// 开始在线识别
		handleOnlineRecogStart() {
			sealVoiceASRModule.recogOnlineStart(
				{
					// appId: '',
					// appKey: '',
					// appSecret: '',
					enableLongSpeech: true
				},
				ret => {
					const resultCode = ret.code;
					console.log('resultCode', resultCode);
					if (resultCode === 1000) {
						modal.toast({
							message: `正在在线识别，开始标识：${resultCode}`,
							duration: 3
						});
						this.recogOnlineBtn = '正在在线识别...';
					} else if (resultCode === 1001) {
						this.recogText += JSON.parse(ret.result).result + ' '
						// uni.showModal({
						// 	content: `获取在线识别结果（${resultCode}）：` + ret.result
						// });
						// modal.toast({
						// 	message: '获取在线识别结果：' + ret.result,
						// 	duration: 3
						// })
					}
				}
			);
		},
		// 结束在线识别
		handleOnlineRecogEnd() {
			// 调用recogOnlineStart接口，传递isFinish为true
			// sealVoiceASRModule.recogOnlineStart({ isFinish: true }, ret => {
			sealVoiceASRModule.recogOnlineEnd({}, ret => {
				const resultCode = ret.code;
				if (resultCode === 1002) {
					modal.toast({
						message: `识别结束，结束标识：${resultCode}`,
						duration: 3
					});
					this.recogOnlineBtn = '开始在线识别';
				}
			});
		}
	}
};
</script>

<style>
.voice-container {
	padding: 10rpx;
}

.voice-container > uni-button {
	margin: 10rpx 0 10rpx 0;
}
</style>
