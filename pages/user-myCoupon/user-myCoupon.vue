<template>
	<view>
		
		<view class="orderNav flexRowBetween borderB1 color6 whiteBj">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">待使用</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">已使用</view>
		</view>
		<view class="orderNavH"></view>
		
		<view class="couponList mglr4" v-show="num==1">
			<view class="item mgt15 whiteBj boxShaow" v-for="(item,index) in couponData" :key="index">
				<view class="topName">
					<view class="bj"><image src="../../static/images/vouchersl-img.png" mode=""></image></view>
					<view class="cont flexRowBetween">
						<view class="icon"><image src="../../static/images/vouchersl-icon.png" mode=""></image></view>
						<view class="title white">
							<view class="fs16">艾灸+推拿+针灸</view>
							<view class="fs12 mgt5">套餐</view>
						</view>
					</view>
				</view>
				<view class="B-infor flexRowBetween">
					<view class="fs13 color6">使用时间：永久有效</view>
					<view class="ewm" @click="ewmShow"><image src="../../static/images/my-order-img.png" mode=""></image></view>
				</view>
			</view>
		</view>
		
		<view class="couponList mglr4" v-show="num==2">
			<view class="item mgt15 whiteBj boxShaow" v-for="(item,index) in couponData" :key="index">
				<view class="topName">
					<view class="bj"><image src="../../static/images/vouchersl-img3.png" mode=""></image></view>
					<view class="cont flexRowBetween">
						<view class="icon"><image src="../../static/images/vouchersl-icon1.png" mode=""></image></view>
						<view class="title white">
							<view class="fs16">艾灸+推拿+针灸</view>
							<view class="fs12 mgt5">套餐</view>
						</view>
					</view>
				</view>
				<view class="B-infor flexRowBetween">
					<view class="fs13 color6">使用时间：永久有效</view>
					<!-- <view class="ewm"><image src="../../static/images/my-order-img.png" mode=""></image></view> -->
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="ewmAlert" v-show="is_ewmShow">
			<view class="closeBtn" @click="ewmShow">×</view>
			<image class="ewm" src="../../static/images/my-order-img.png" mode=""></image>
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
				num:1,
				is_show:false,
				couponData:2,
				is_ewmShow:false
			}
		},
		
		onLoad() {
			const self = this;
		},
		
		methods: {
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
					
				}
			},
			ewmShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	page{background: #F5F5F5;}
	.orderNav{position: fixed;left: 0;top: 0;right: 0;}
	.orderNavH{height: 80rpx;}
	image{width: 100%;height: 100%;display: block;}
	
	.couponList .item{width: 100%;height: 294rpx;box-sizing: border-box;border-radius: 10rpx;overflow: hidden;}
	.topName{position: relative;height: 160rpx;padding: 0 40rpx;}
	.topName .bj{position: absolute;left: 0;top: 0;right: 0;bottom: 0;}
	.topName .cont{position: relative;z-index: 2;height: 148rpx;}
	.topName .cont .icon{width: 90rpx;height: 90rpx;}
	.topName .cont .title{text-align: right;}
	.couponList .item .ewm{width: 80rpx;height: 80rpx;}
	.B-infor{height: 134rpx;padding: 0 40rpx;}
	
	.ewmAlert{position: fixed;top: 40%;left: 50%;transform: translateX(-50%);z-index: 45;}
	.ewmAlert .ewm{width: 360rpx;height: 360rpx;display: block;}
	
</style>
