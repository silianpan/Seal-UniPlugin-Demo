<template>
	<view class="uni-container">
		<view class="uni-panel" v-for="(item, index) in list" :key="item.id">
			<view class="uni-panel-h" :class="item.open ? 'uni-panel-h-on' : ''" @click="triggerCollapse(index)">
				<text class="uni-panel-text">{{item.name}}</text>
			</view>
			<view class="uni-panel-c" v-if="item.open">
				<view class="uni-navigate-item" v-for="(item2,key) in item.pages" :key="key" @click="goDetailPage(item2.url)">
					<text class="uni-navigate-text">{{item2.name ? item2.name : item2}}</text>
				</view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				list: [{
					id: 'seal-officeonline',
					name: 'Seal-OfficeOnline',
					open: false,
					url: '/pages/demo/seal-officeonline'
				},
				{
					id: 'seal-imagevideo',
					name: 'Seal-ImageVideo',
					open: false,
					url: '/pages/demo/seal-imagevideo'
				},
				{
					id: 'seal-ocr',
					name: 'Seal-OCR',
					open: false,
					url: '/pages/demo/seal-ocr'
				}],
				navigateFlag: false
			}
		},
		onLoad() {},
		methods: {
			triggerCollapse(e) {
				if (!this.list[e].pages) {
					this.goDetailPage(this.list[e].url);
					return;
				}
				for (var i = 0; i < this.list.length; ++i) {
					if (e === i) {
						this.list[i].open = !this.list[e].open;
					} else {
						this.list[i].open = false;
					}
				}
			},
			goDetailPage(e) {
				if (this.navigateFlag) {
					return;
				}
				this.navigateFlag = true;
				uni.navigateTo({
					url: e
				});
				setTimeout(() => {
					this.navigateFlag = false;
				}, 200)
				return false;
			}
		}
	}
</script>
