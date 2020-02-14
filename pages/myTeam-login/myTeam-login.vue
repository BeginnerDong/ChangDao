<template>
	<view v-if="showAll">
		<view><image style="width: 100%;height: 346rpx;display: block;" src="../../static/images/the-login-img.png" mode="widthFix"></image></view>
		
		<view>
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">账号：</view>
					<view class="rr">
						<input type="text" v-model="submitData.login_name" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">密码：</view>
					<view class="rr">
						<input type="password" v-model="submitData.password" placeholder="请输入密码" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="btn" type="button" @click="submit">登录</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if (uni.getStorageSync('staffToken')&&uni.getStorageSync('staffInfo').user_type==1) {
				uni.redirectTo({
					url: '/pages/myTeam/myTeam'
				})
			}else{
				self.showAll = true
			}
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('staffToken', res.token);
							uni.setStorageSync('staffInfo', res.info);
							uni.redirectTo({
								url: '/pages/myTeam/myTeam'
							}) 
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.login(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
		},
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

