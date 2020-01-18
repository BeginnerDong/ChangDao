<template>
	<view>
		<view class="whiteBj servicer mgt15">
			<view class="fs13 color9">选择服务者</view>
			<view class="cont">
				<view class="lis flexRowBetween" v-for="(item,index) in servicerData" :key="index">
					<view class="flex" @click="seltServicer(index)">
						<image class="seltIcon" :src="seltNum==index?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image>
						<view>张月</view>
					</view>
					<view class="flexEnd" @click="Router.navigateTo({route:{path:'/pages/staffDetail/staffDetail'}})"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
				</view>
			</view>
		</view>
	
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button">确认</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				seltNum:0,
				servicerData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			seltServicer(index){
				const self = this;
				self.seltNum = index
			}
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 60rpx;}
	.servicer{padding: 30rpx 4%;}
	.servicer .lis{margin-top: 30rpx;}
	.servicer .lis .seltIcon{width: 30rpx;height: 30rpx;margin-right: 20rpx;}
	
</style>
