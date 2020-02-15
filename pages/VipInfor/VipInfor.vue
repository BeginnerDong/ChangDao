<template>
	<view>
		
		<view class="mglr4 vipInfor flexColumn center">
			<view><image class="pic" src="../../static/images/members-img.png" mode=""></image></view>
			<view class="fs13 color6 text">在本店铺预存定量金额成为本店会员，享受服务和产品的优惠折扣，享受佣金以及返利收益</view>
			<view class="adrs flexColumn">
				<view class="item">
					<image class="icon" src="../../static/images/members-icon.png" mode=""></image>
					<view>{{mainData.description}}</view>
				</view>
				<view class="item">
					<image class="icon" src="../../static/images/members-icon1.png" mode=""></image>
					<view>{{mainData.url}}</view>
				</view>
			</view>
		</view>
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
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					title:'商家信息',
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.labelGet(postData, callback);
			},
		}
	};
</script>

<style>
	.vipInfor{padding: 130rpx 7% 60rpx 7%;}
	.pic{width: 300rpx;height: 270rpx; display: block;}
	.text{line-height: 48rpx;margin: 60rpx 0 80rpx 0;}
	
	.adrs .item{padding-left: 50rpx;position: relative;margin-bottom: 40rpx;}
	.adrs .item .icon{width: 38rpx;height: 38rpx;display: block;position: absolute;top: 0;left: 0;}
	
</style>
