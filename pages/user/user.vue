<template>
	<view>
		
		<view class="userHead pdlr4">
			<view class="infor flex">
				<view class="photo"
				style="width: 120rpx;height: 120rpx;border-radius: 50%;overflow: hidden;">
					<open-data type="userAvatarUrl"></open-data>
				</view>
				
				<view style="width: 70%;">
					<view class="fs14 pdb5"><open-data type="userNickName"></open-data></view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myInfor/user-myInfor'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon.png" mode=""></image>
					<view class="">个人信息</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-myBalance/user-myBalance'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon1.png" mode=""></image>
					<view class="">我的余额</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">收货地址</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-orderInfor/user-orderInfor'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view class="">订单信息</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-login/myTeam-login'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view class="">我的团队</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-mySpread/user-mySpread'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
					<view class="">我的推广</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">理疗</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/product/product'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">产品</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 130rpx;}
	.userHead{box-sizing: border-box;padding: 50rpx 4%;}
	.userHead .photo{width: 120rpx;height: 120rpx;border-radius: 50%;margin-right: 20rpx;border: 2px solid #ffecca;;box-sizing: border-box;}
	
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
</style>
