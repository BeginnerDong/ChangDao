<template>
	<view>
		
		<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15">
			<view class="tt" :class="serviceNum==1?'on':''" @click="serviceChange('1')">提供服务</view>
			<view class="tt" :class="serviceNum==2?'on':''" @click="serviceChange('2')">预约待服务</view>
		</view>
		<view class="mglr4">
			<view class="proRow" v-show="serviceNum==1">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color6">时间：2020-01-18</view>
						<view class="red">已核销</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">按摩精油</view>
							<view class="B-price flexRowBetween">
								<view class="price">56.00</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
					<view class="dashedB1 mgt15"></view>
					<view class="adviseF5 fs12">
						<span class="ftw">健康管理意见：</span>是的解放路开发和公交卡发烧了连霍高速换个飞机卡死了个很费时间开了个会挂号费就开始都发的及共和见客户
					</view>
				</view>
			</view>
			<view class="proRow" v-show="serviceNum==2">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color6">时间：2020-01-18</view>
						<view class="red">待核销</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">按摩精油</view>
							<view class="B-price flexRowBetween">
								<view class="price">56.00</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
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
				score:'',
				wx_info:{},
				curr:1,
				serviceNum:1,
				is_show:false,
				proNum:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			},
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum
				}
			},
			proChange(proNum){
				const self = this;
				if(proNum!= self.proNum){
					self.proNum = proNum
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;}
	.kefuIcon{width: 34rpx;height: 36rpx; display: block;}
	.phoneIcon{width: 27rpx;height: 34rpx; display: block;}
	.sEwm{width: 100rpx;height: 100rpx; display: block;}
	.LRbet .line{padding-top: 30rpx;}
	.LRbet .ll{width: 20%;}
	.LRbet .rr{width: 80%;}
	.adviseF5{padding: 30rpx;background: #f5f5f5;}
</style>
