<template>
	<view>
		<view><image style="width: 100%;height: 346rpx;display: block;" src="../../static/images/the-login-img.png" mode="widthFix"></image></view>
		
		<view>
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">账号：</view>
					<view class="rr">
						<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">密码：</view>
					<view class="rr">
						<input type="password" value="" placeholder="请输入密码" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/user/user'}})">登录</button>
			</view>
			<view class="flex pdt5" style="width: 80%;margin: 0 auto;">
				<view class="color9 fs12" @click="Router.navigateTo({route:{path:'/pages/register/register'}})">没有账号，去注册</view>
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
				wx_info:{},
				is_show:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
@import "../../assets/style/orderNav.css";	
.orderNav .tt{width: 50%;}
.orderNav .tt.on::after{ width: 200rpx;}

.loginEdit{padding:80rpx 4% 0 4%;}
.loginEdit .item{line-height: 60rpx;box-sizing: border-box;padding-top: 40rpx;}
.loginEdit .item .ll{width: 18%;font-size: 30rpx;}
.loginEdit .item .icon{width: 44rpx; height: 44rpx;}
.loginEdit .item .rr{width: 82%;font-size: 26rpx;padding:16rpx 20rpx;border-bottom: 1px solid #eee;box-sizing: border-box;}
.loginEdit .item .rr input{width: 100%;height: 40rpx;line-height: 40rpx; display: block;font-size: 24rpx;}
.loginEdit .item .rr .placeholder{font-size: 26rpx;}
</style>

