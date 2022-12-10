<template>
	<view class="ocr-container">
		<u-button plain type="primary" @click="handleOcr('general_basic')">通用文字识别</u-button>
		<u-button plain type="primary" @click="handleOcr('accurate_basic')">通用文字识别（高精度版）</u-button>
		<u-button plain type="primary" @click="handleOcr('general')">通用文字识别（含位置信息版）</u-button>
		<u-button plain type="primary" @click="handleOcr('accurate')">通用文字识别（高精度含位置信息版）</u-button>
		<u-button plain type="primary" @click="handleOcr('general_enhanced')"><span
				style="font-size: 20rpx">通用文字识别（含生僻字版，该服务已经停止，改用高精度版/高精度含位置版）</span></u-button>
		<u-button plain type="primary" @click="handleOcr('general_webimage')">网络图片文字识别</u-button>

		<!-- 身份证识别 -->
		<u-button plain type="primary" @click="handleOcr('idcard_front', 1)">身份证正面拍照识别</u-button>
		<u-button plain type="primary" @click="handleOcr('idcard_back', 2)">身份证反面拍照识别</u-button>
		<u-button plain type="primary" @click="handleOcr('idcard_front_scan')">身份证正面（嵌入式质量控制+云端识别）</u-button>
		<u-button plain type="primary" @click="handleOcr('idcard_back_scan')">身份证反面（嵌入式质量控制+云端识别）</u-button>

		<!-- 银行卡识别 -->
		<u-button plain type="primary" @click="handleOcr('bankcard')">银行卡识别</u-button>
		<u-button plain type="primary" @click="handleOcr('bankcard_scan')">银行卡采集识别</u-button>

		<u-button plain type="primary" @click="handleOcr('businesscard')">名片识别</u-button>
		<u-button plain type="primary" @click="handleOcr('driving_license')">驾驶证识别</u-button>
		<u-button plain type="primary" @click="handleOcr('vehicle_license')">行驶证识别</u-button>
		<u-button plain type="primary" @click="handleOcr('license_plate')">车牌识别</u-button>

		<u-button plain type="primary" @click="handleOcr('business_license')">营业执照识别</u-button>
		<u-button plain type="primary" @click="handleOcr('receipt')">通用票据识别</u-button>
		<u-button plain type="primary" @click="handleOcr('vatinvoice')">增值税发票识别</u-button>

		<u-button plain type="primary" @click="handleOcr('lottery')">彩票识别</u-button>
		<u-button plain type="primary" @click="handleOcr('taxireceipt')">出租车票</u-button>
		<u-button plain type="primary" @click="handleOcr('vincode')">VIN码</u-button>
		<u-button plain type="primary" @click="handleOcr('trainticket')">火车票</u-button>

		<u-button plain type="primary" @click="handleOcr('numbers')">数字识别</u-button>
		<u-button plain type="primary" @click="handleOcr('qrcode')">二维码识别</u-button>
		<u-button plain type="primary" @click="handleOcr('trip_ticket')">飞机行程单识别</u-button>
		<u-button plain type="primary" @click="handleOcr('car_sell_invoice')">机动车销售发票</u-button>
		<u-button plain type="primary" @click="handleOcr('vihicle_sertification')">车辆合格证</u-button>
		<u-button plain type="primary" @click="handleOcr('example_doc_reg')">试卷分析和识别</u-button>
		<u-button plain type="primary" @click="handleOcr('written_text')">手写文字识别</u-button>

		<u-button plain type="primary" @click="handleOcr('passport')">护照识别</u-button>
		<u-button plain type="primary" @click="handleOcr('hukou_page')">户口本识别</u-button>

		<u-button plain type="primary" @click="handleOcr('normal_machine_invoice')">普通机打发票识别</u-button>
		<u-button plain type="primary" @click="handleOcr('weight_note')">磅单识别</u-button>
		<u-button plain type="primary" @click="handleOcr('medical_detail')">医疗费用明细识别</u-button>
		<u-button plain type="primary" @click="handleOcr('online_taxi_itinerary')">网约车行程单识别</u-button>
		<u-button plain type="primary" @click="handleOcr('waybill')">快递面单识别</u-button>

		<u-button plain type="primary" @click="handleOcr('custom')">自定义模板识别</u-button>
		
		<!-- 身份证识别图片 -->
		<view>身份证正面</view>
		<image v-if="idCardFrontImg" :src="idCardFrontImg"></image>
		<view>身份证反面</view>
		<image v-if="idCardBackImg" :src="idCardBackImg"></image>
	</view>
</template>

<script>
	const sealOcrModule = uni.requireNativePlugin('Seal-OCR')
	export default {
		data() {
			return {
				idCardFrontImg: null,
				idCardBackImg: null
			}
		},
		methods: {
			handleOcr(ocrType, idCardFlag) {
				sealOcrModule.ocr({
						ak: '',
						sk: '',
						ocrType,
						resultType: 2,
						scaleWidth: 0.1,
						scaleHeight: 0.1,
						albumEnable: false
					},
					res => {
						// uni.showModal({
						// 	content: '获取识别结果：' + JSON.stringify(res)
						// })
						console.log('res', res)
						if (idCardFlag === 1) {
							this.idCardFrontImg = 'file://' + res.ocrImagePath
						} else if (idCardFlag === 2) {
							this.idCardBackImg = 'file://' + res.ocrImagePath
						}
						uni.showToast({
							title: '获取识别结果：' + JSON.stringify(res),
							icon: "none",
							duration: 10000
						})
					})
			}
		}
	}
</script>

<style>
	.ocr-container {
		padding: 10rpx;
	}

	.ocr-container>uni-button {
		margin: 10rpx 0 10rpx 0;
	}
</style>
