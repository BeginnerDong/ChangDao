<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="proRow">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">按摩精油</view>
							<view class="B-price flexRowBetween">
								<view class="price">889.00</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>	
			
			<view class="pdlr4 pdt15 pdb15 flexRowBetween whiteBj">
				<view>时间</view>
				<view class="fs12 color6 flexEnd"><view>请选择</view><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			
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
		</view>
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14">56</view></view>
			<view class="payBtn" @click="PayShow">提交订单</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="PayShow" v-show="is_PayShow">
			<view class="closebtn" @click="PayShow">×</view>
			<view class="center ftw fs22 red">￥56</view>
			<view class="flexCenter mgt10">
				<view class="countDown">请在<span class="red">19:57</span>内完成支付</view>
			</view>
			<view class="seltLis fs13">
				<view class="item flexRowBetween" @click="changePay('1')">
					<view class="flex">
						<image class="icon" src="../../static/images/the-order-icon3.png" mode=""></image>
						<view>余额支付</view>
					</view>
					<view><image class="seltIcon" :src="payCurr==1?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="changePay('2')">
					<view class="flex">
						<image class="icon" src="../../static/images/the-order-icon4.png" mode=""></image>
						<view>微信支付</view>
					</view>
					<view><image class="seltIcon" :src="payCurr==2?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image></view>
				</view>
			</view>
			<view class="submitbtn" style="margin-top:80rpx ;">
				<button class="btn" type="submint">立即支付</button>
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
				score:'',
				wx_info:{},
				is_show:false,
				seltNum:0,
				servicerData:[{},{},{},{},{},{}],
				payCurr:1,
				is_PayShow:false
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
			},
			PayShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_PayShow = !self.is_PayShow;
			},
			changePay(payCurr){
				const self = this;
				if(payCurr!=self.payCurr){
					self.payCurr = payCurr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/detail.css";
	page{background: #F5F5F5;}
	.servicer{padding: 30rpx 4%;}
	.servicer .cont{max-height: 500rpx;overflow-y: auto;}
	.servicer .lis{margin-top: 30rpx;}
	.servicer .lis .seltIcon{width: 30rpx;height: 30rpx;margin-right: 20rpx;}
	.PayShow{padding: 50rpx 4% 80rpx 4%;position:fixed; left: 0;right: 0;bottom: 0;z-index: 45;background: #fff;}
	.countDown{line-height: 50rpx;background: #F5F5F5;padding: 0 30rpx;font-size: 24rpx; color: #999;border-radius: 30rpx;}
	.seltLis{margin-top: 60rpx;}
	.seltLis .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.seltLis .item .icon{width: 40rpx;height: 40rpx; display: block;margin-right: 20rpx;}
	.seltLis .item .seltIcon{width: 36rpx;height: 36rpx;}
</style>
