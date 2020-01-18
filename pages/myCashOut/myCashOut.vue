<template>
	<view>
		
		<view class="tixianBox radius10">
			<view class="flexRowBetween bankmsg addBtn" >
				<view>到账银行卡</view>
				<view class="flexCenter" @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
					<text class="color9 fs13">去添加</text>
					<image class="arrow arrowR" src="../../static/images/the-order-icon2.png" mode=""></image>
				</view>
			</view>
		
			<view class="flexRowBetween bankmsg addBtn">
				<view>到账银行卡</view>
				<view class="lft fs12">
					中国银行
					<view class="color6 num">(2356)</view>
				</view>
				<view @click="Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})">
					<image class="arrow arrowR" src="../../static/images/the-order-icon2.png" mode=""></image>
				</view>
			</view>
		
			<view class="editMoney">
				<view class="tit">提现金额</view>
				<view class="input">
					<input type="number" value="" placeholder="请输入提现金额" placeholder-class="placeholder">
				</view>
				<view class="flexRowBetween fs12 color6">
					<view>可提现金额200元</view>
					<view class="color2">全部提现</view>
				</view>
				<view class="submitbtn" style="margin-top: 100rpx;">
					<button class="btn" type="button">提现</button>
				</view>
				<view class="color9 fs12" style="margin:10rpx 10% 30rpx 10%;">提现金额大于300元要扣除税金</view>
				
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
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>

<style>
	/* @import "../../assets/style/common.css"; */
	page{ background: #f5f5f5;}
	
	.tixianBox{margin:100rpx 4%;background: #fff; }
	
	.bankmsg .lft{ display: flex; width: 50%; justify-content: flex-start; align-items: center; font-size: 26rpx;color: #999;}
	.bankmsg .lft .num{ margin-left: 20rpx; font-size: 26rpx;}
	
	.addBtn{border-radius: 10rpx; height: 100rpx; font-size: 30rpx;padding: 0 30rpx;}
	.addBtn .arrow{ margin-left: 20rpx;}
	
	.editMoney{padding: 30rpx;}
	.editMoney .tit{ font-size: 28rpx; line-height: 80rpx;}
	.editMoney .input{ display: flex; justify-content: flex-start; line-height: 100rpx; margin-bottom: 30rpx;border-bottom: 2rpx solid #e7e7e7;}
	.editMoney .input input{ font-size: 46rpx; line-height: 100rpx; display: block; width: 80%; height: 100rpx; box-sizing: border-box;}
	.editMoney .input .placeholder{font-size: 30rpx;}
	.editMoney .input::before{content: "￥"; font-size: 50rpx; margin-right: 10rpx;}
</style> 
