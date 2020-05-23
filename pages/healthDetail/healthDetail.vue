<template>
	<view>
		
		
		<view class="mglr4 xqInfor">
			<view class="center pdtb15 fs15">{{mainData.title}}</view>
			<view class="cont fs13">
				<view class="content ql-editor" style="padding:0;"
				v-html="mainData.content">
				</view>
			</view>
		</view>
		
		<view class="FxIcon"><image src="../../static/images/health-information-icon.png" mode=""></image></view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;margin:20rpx auto;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	page{padding-bottom:60rpx;}
	.FxIcon{position: fixed;top: 20%;right: 30rpx;}
	.FxIcon image{width: 90rpx;height: 90rpx;display: block;}
</style>
