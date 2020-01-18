<template>
	<view>
		
		<view class="userHead pdlr4">
			<view class="infor flex">
				<image class="photo" src="../../static/images/about-img.png" mode=""></image>
				<view style="width: 70%;">
					<view class="fs14 pdb5">张月</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-changePswd/myTeam-changePswd'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon.png" mode=""></image>
					<view class="">修改密码</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-service/myTeam-service'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
					<view class="">服务</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-YongJin/myTeam-YongJin'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">我的佣金</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-mySpread/myTeam-mySpread'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view class="">我的推广</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-scan/myTeam-scan'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view class="">扫一扫</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
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
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 130rpx;}
	.userHead{box-sizing: border-box;padding: 50rpx 4%;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;border: 2px solid #ffecca;;box-sizing: border-box;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
</style>
