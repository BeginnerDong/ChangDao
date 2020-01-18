<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="curr==1?'on':''" @click="changeCurr('1')">服务订单</view>
				<view class="item" :class="curr==2?'on':''" @click="changeCurr('2')">产品订单</view>
			</view>
		</view>
		
		<view v-show="curr==1">
			<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15">
				<view class="tt" :class="serviceNum==1?'on':''" @click="serviceChange('1')">全部</view>
				<view class="tt" :class="serviceNum==2?'on':''" @click="serviceChange('2')">待使用</view>
				<view class="tt" :class="serviceNum==3?'on':''" @click="serviceChange('3')">已使用</view>
				<view class="tt" :class="serviceNum==4?'on':''" @click="serviceChange('4')">已评价</view>
			</view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待付款</view>
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
						<view class="LRbet fs12">
							<view class="line pdt15 flexRowBetween">
								<view class="ll">在线客服</view>
								<view class="rr flexEnd"><image class="kefuIcon" src="../../static/images/my-order-icon.png" mode=""></image></view>
							</view>
							<view class="line pdt15 flexRowBetween">
								<view class="ll">地址</view>
								<view class="rr flexEnd">陕西省西安市长安区韦曲街道</view>
							</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待使用</view>
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
						<view class="LRbet fs12">
							<view class="line pdt15 flexRowBetween">
								<view class="ll">联系电话</view>
								<view class="rr flexEnd">
									<view>15623563256</view>
									<image class="phoneIcon mgl10" src="../../static/images/my-order-icon1.png" mode=""></image></view>
							</view>
							<view class="line pdt15 flexRowBetween">
								<view class="ll">地址</view>
								<view class="rr flexEnd">陕西省西安市长安区韦曲街道</view>
							</view>
							<view class="line pdt15 flexRowBetween">
								<view class="ll"></view>
								<view class="rr flexEnd">
									<image class="sEwm" src="../../static/images/my-order-img.png" mode=""></image>
								</view>
							</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待使用</view>
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
						<view class="underBtn flexEnd mgt15">
							<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user-orderPingJia/user-orderPingJia'}})">去评价</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待使用</view>
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
						<view class="adviseF5 fs12 mgt15">
							<span class="ftw">评价：</span>是的解放路开发和公交卡发烧了连霍高速换个飞机卡死了个很费时间开了个会挂号费就开始都发的及共和见客户
						</view>
					</view>
				</view>
			</view>
		</view>
		<view v-show="curr==2">
			<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15">
				<view class="tt" :class="proNum==1?'on':''" @click="proChange('1')">全部</view>
				<view class="tt" :class="proNum==2?'on':''" @click="proChange('2')">待付款</view>
				<view class="tt" :class="proNum==3?'on':''" @click="proChange('3')">待收货</view>
				<view class="tt" :class="proNum==4?'on':''" @click="proChange('4')">已收货</view>
				<view class="tt" :class="proNum==5?'on':''" @click="proChange('5')">已评价</view>
			</view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待付款</view>
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
						<view class="underBtn flexEnd mgt15">
							<view class="Bbtn">去支付</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">待收货</view>
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
						<view class="underBtn flexEnd mgt15">
							<view class="Bbtn">确认收货</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已收货</view>
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
						<view class="underBtn flexEnd mgt15">
							<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user-orderPingJia/user-orderPingJia'}})">去评价</view>
						</view>
					</view>
					<view class="item">
						<view class="fs12 flexRowBetween mgb10">
							<view class="color9">交易时间：2020-01-18</view>
							<view class="red">已评价</view>
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
						<view class="adviseF5 fs12 mgt15">
							<span class="ftw">评价：</span>是的解放路开发和公交卡发烧了连霍高速换个飞机了个很费时间开了个会挂号费就开始都发的及共和见客户
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
