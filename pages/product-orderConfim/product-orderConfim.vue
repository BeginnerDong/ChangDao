<template>
	<view>
		<view class="mglr4 mgt15">
			
			<view class="pdlr4 pdt15 pdb15 whiteBj radius10 fs13">
				<view class="flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="color6 ">收货地址</view>
					<view class=""><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
				</view>
				<view class="">
					<view class="flex pdt10 pdb5">
						<view class="mgr15">张三</view>
						<view>15623230565</view>
					</view>
					<view>陕西省西安市长安区韦曲街道</view>
				</view>
			</view>
			
			<view class="proRow mgt15">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">按摩精油</view>
							<view class="B-price flexRowBetween">
								<view class="price">889.00</view>
								<view class="">
									<view class="numBox flex">
										<view @click="counter('-')">-</view>
										<view class="num color6">{{count}}</view>
										<view @click="counter('+')">+</view>
									</view>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14">56</view></view>
			<view class="payBtn" @click="payVipShow">提交订单</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		
		<!-- 不是会员购买会员弹框 -->
		<view class="payVipShow" v-show="is_payVipShow">
			<view class="flexCenter">
				<image class="icon" src="../../static/images/members-img.png"></image>
			</view>
			<view class="pdt15 pdb20 fs12" style="line-height: 44rpx;">您现在还不是会员，建议您购买会员，享受服务和产品的优惠折扣，享受佣金以及返利收益</view>
			<view class="submitbtn">
				<view class="Wbtn" style="border-radius: 10rpx;" @click="Router.navigateTo({route:{path:'/pages/VipInfor/VipInfor'}})">购买</view>
			</view>
			<view class="mgt20">
				<view class="center pdtb10 color6" @click="PayShow">暂不购买</view>
			</view>
		</view>
		
		<!-- 支付方式弹框 -->
		<view class="PayShow" v-show="is_PayShow">
			<view class="closebtn" @click="PayShowClose">×</view>
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
				payCurr:1,
				is_PayShow:false,
				count:1,
				is_payVipShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
			},
			payVipShow(){
				const self = this;
				self.is_show = true
				self.is_payVipShow = !self.is_payVipShow;
			},
			PayShow(){
				const self = this;
				self.is_payVipShow = false;
				self.is_PayShow = true;
			},
			PayShowClose(){
				const self = this;
				self.is_show = false;
				self.is_PayShow = false;
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
	.PayShow{padding: 50rpx 4% 80rpx 4%;position:fixed; left: 0;right: 0;bottom: 0;z-index: 45;background: #fff;}
	.countDown{line-height: 50rpx;background: #F5F5F5;padding: 0 30rpx;font-size: 24rpx; color: #999;border-radius: 30rpx;}
	.seltLis{margin-top: 60rpx;}
	.seltLis .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.seltLis .item .icon{width: 40rpx;height: 40rpx; display: block;margin-right: 20rpx;}
	.seltLis .item .seltIcon{width: 36rpx;height: 36rpx;}
	
	/* 数量加减 */
	.numBox{border:1px solid #eee; border-radius: 8rpx;}
	.numBox view{ width: 50rpx; height: 44rpx;color: #999; font-size:32rpx ; text-align: center; line-height: 44rpx;}
	.numBox view.num{ width: 60rpx; font-size: 26rpx;border-left:1px solid #eee;border-right:1px solid #eee; color: #666;}
	
	/* 不是会员购买弹框 */
	.payVipShow{width: 80%;border-radius: 10rpx;background: #fff;padding: 60rpx 50rpx;box-sizing: border-box;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 45;}
	.payVipShow .icon{width: 220rpx;height: 198rpx; }
</style>
