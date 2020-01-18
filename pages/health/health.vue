<template>
	<view>
		
		<view class="mglr4 healthList">
			<view class="item flexRowBetween radius10" v-for="(item,index) in healthData" :key="index" @click="Router.navigateTo({route:{path:'/pages/healthDetail/healthDetail'}})">
				<view class="ll"><image src="../../static/images/health-img.png" mode=""></image></view>
				<view class="rr">
					<view class="tit avoidOverflow">王氏按摩服务健康标题王氏按摩服务健康标题</view>
					<view class="text fs12 color6 avoidOverflow2">内容内容内容内容内容内容内容内容内容内容内容内容内容内容</view>
					
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
				showView: false,
				wx_info:{},
				is_show:false,
				healthData:[{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	
	page{padding-bottom:60rpx;}
	.healthList .item{padding: 30rpx 0;align-items: flex-start;border-bottom: 1px solid #eee;}
	.healthList .ll{width: 260rpx;height: 200rpx;overflow: hidden;border-radius: 10rpx;}
	.healthList .ll image{width: 100%;height: 100%; display: block;}
	.healthList .rr{width: 60%;height: 200rpx;padding: 10rpx 0;box-sizing: border-box;position: relative;}
	.healthList .rr .text{line-height: 36rpx;margin-top: 80rpx;position: absolute;left: 0;right: 0;bottom: 16rpx;}
</style>
