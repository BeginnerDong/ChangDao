<template>
	<view>
			<view class="myaddress-lis" v-for="(item,index) in myaddressDate" :key="index">
				<view class="name">张三<view class="numb">15632564562</view></view>
				<view class="adrs">陕西省西安市高新区大都荟</view>
				<view class="seltBox">
					<view class="L"  @click="seltCurr(index)">
						<image class="icon" :src="curr == index?'../../static/images/address-icon2.png':'../../static/images/address-icon3.png'"  mode=""></image>
						默认地址
					</view>
					<view class="R fs12 color9 flexEnd">
						<view class="child flexEnd mgr20" @click="Router.navigateTo({route:{path:'/pages/user_addressAdd/user_addressAdd'}})"><image src="../../static/images/address-icon.png" mode=""></image>编辑</view>
						<view class="child flexEnd" @click="deltAlert"><image src="../../static/images/address-icon1.png" mode=""></image>删除</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/user_addressAdd/user_addressAdd'}})">添加新地址</button>
			</view>
			
			<!-- 删除弹框 -->
			<view class="seltAlert" v-if="is_show">
				<view class="infor radius10 center">
					<view class="closebtn"  @click="deltAlert">×</view>
					<view class="tit font16" style="padding-bottom: 60rpx;">确认是否删除这个地址</view>
					<view class="btnB flexRowBetween">
						<view>是</view>
						<view  class="on" @click="deltAlert">否</view>
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
				is_show:false,
				wx_info: {},
				curr:0,
				myaddressDate:[{},{}]
			}
		},

		onLoad(options) {
			uni.setStorageSync('canClick', true);
		},

		onShow() {
			const self = this;
			document.title = ''
		},

		methods: {
			seltCurr(index){
				const self = this;
				self.curr = index
			},
			deltAlert(){
				const self = this;
				self.is_show=!self.is_show;
			},
			getMainData() {
				const self = this;
				self.$apis.userGet(postData, callback);
			}
		}
	}
</script>

<style>
	@import "../../assets/style/seltAlert.css";
	page{ background: #f5f5f5;}
	.myaddress-lis{padding:30rpx 4%;margin-bottom: 20rpx;background: #fff; margin-bottom: 30rpx;}
	.myaddress-lis .name{ font-size: 28rpx; color: #222;}
	.myaddress-lis .numb{margin-left: 30rpx; width: 300rpx; display: inline-block;}
	.myaddress-lis .adrs{ font-size: 26rpx;color: #999; line-height: 40rpx;padding: 10rpx 0;}
	.seltBox{ display: flex;justify-content: space-between; align-items: center;padding-top: 10rpx;}
	.seltBox .L{ width: 30%; display: flex; align-items: center; font-size: 24rpx; color: #999;}
	.seltBox .L .icon{ width: 30rpx; height: 30rpx; display: inline-block; margin-right: 10rpx;}
	.seltBox .R{ width: 70%;}
	.seltBox .R image{  width:30rpx; height: 30rpx; display:block; margin-right: 8rpx;}

</style>
