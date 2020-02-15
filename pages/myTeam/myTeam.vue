<template>
	<view>
		
		<view class="userHead pdlr4">
			<view class="infor flex">
				<image class="photo" :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
				<view style="width: 70%;">
					<view class="fs14 pdb5">{{mainData.name}}</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-changePswd/myTeam-changePswd'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/team-icon1.png" mode=""></image>
					<view class="">修改密码</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-service/myTeam-service'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/team-icon2.png" mode=""></image>
					<view class="">服务</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-YongJin/myTeam-YongJin'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/team-icon3.png" mode=""></image>
					<view class="">我的佣金</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/myTeam-mySpread/myTeam-mySpread'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/team-icon4.png" mode=""></image>
					<view class="">我的推广</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="scanCode">
				<view class="ll flex">
					<image class="icon" src="../../static/images/team-icon5.png" mode=""></image>
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
				mainData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			scanCode(){
				const self = this;
				uni.scanCode({
				    success: function (res) {
				        self.Router.navigateTo({route:{path:'/pages/myTeam-scan/myTeam-scan?id='+res.result}})
				        console.log('条码内容：' + res.result);
				    }
				});
			},
			
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getStaffToken'
				postData.searchItem.user_no = uni.getStorageSync('staffInfo').user_no		
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						uni.setNavigationBarTitle({
						    title: self.mainData.name
						});
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},

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
