<template>
	<view>
		
		<view class="mglr4 xqInfor" style="padding-bottom: 240rpx;">
			<view class="center pdtb15 fs15">{{mainData.title}}</view>
			<view class="cont fs13">
				<view class="content ql-editor" style="padding:0;"
				v-html="mainData.content">
				</view>
			</view>
		</view>
		<!-- <view><image style="width: 100%;height: 346rpx;display: block;" src="../../static/images/the-login-img.png" mode="widthFix"></image></view>
		
		<view>
			<view class="loginEdit">
				<view class="item flex">
					<view class="ll">账号：</view>
					<view class="rr">
						<input type="text" v-model="submitData.phone" maxlength="11" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">验证码：</view>
					<view class="rr flexRowBetween">
						<view style="width: 40%;"><input type="number"  v-model="submitData.smsCode" placeholder="请输入验证码" placeholder-class="placeholder" /></view>
						
						<view class="flexEnd pubColor" @click="sendCode()" v-if="!hasSend">{{text}}</view>
						<view class="flexEnd pubColor"  v-else>{{text}}</view>
					</view>
				</view>
				
				<view class="item flex">
					<view class="ll">密码：</view>
					<view class="rr">
						<input type="password" v-model="submitData.password" placeholder="请输入密码" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top:110rpx;">
				<button class="btn" type="button"  open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">注册</button>
			</view>
			<view class="submitbtn" style="margin-top:20rpx;">
				<button class="btn" type="button" @click="Router.reLaunch({route:{path:'/pages/index/index'}})">返回首页</button>
			</view>
		</view> -->
		<view style="position: fixed;bottom: 20px;width: 100%;">
			<button open-type="getPhoneNumber" @getphonenumber="getPhoneNumber" type="primary" style="color: #fff;border-radius: 0;width: 80%;margin-left:auto ;margin-right: auto;;font-size:15px">
				微信用户快捷登录
			</button>
			<button @click="Router.reLaunch({route:{path:'/pages/index/index'}})" style="margin-top: 50rpx;border-radius: 0;width: 80%;margin-left:auto ;margin-right: auto;font-size:15px">
				暂不登录
			</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				submitData:{
					phone:'',
					password:'',
					smsCode:''
				},
				currentTime:61,
				text:'获取验证码',
				hasSend:false,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				console.log(2323)
				const postData = {};
				postData.searchItem = {
					thirdapp_id:2
				};
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['广告']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getPhoneNumber(e) {
				const self = this;
				console.log('e', e);
				if (e.detail.errMsg == 'getPhoneNumber:fail user deny') {
					return
				};
				const postData = {
					appid: uni.getStorageSync('user_info').thirdApp.appid,
					tokenFuncName: 'getProjectToken',
					encryptedData: e.detail.encryptedData,
					iv: e.detail.iv
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.userInfoUpdate(res.info.phoneNumber)
					};
				}
				self.$apis.decryptWxInfo(postData, callback)
			},
			
			
			sendCode(){
				var self = this;
				console.log(111)
				if(self.hasSend){
					return;
				};
				var phone = self.submitData.phone;
				
				if (phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(phone)) {
					self.$Utils.showToast('请输入正确的手机号', 'none', 1000)
					
					return;
				}
				var postData = {
					data:{
						phone:self.submitData.phone,
					
					}
				};
				var callback = function(res){
					if(res.solely_code==100000){
						self.hasSend = true;
						var interval = setInterval(function() {
							self.currentTime--; //每执行一次让倒计时秒数减一
						
							self.text=self.currentTime + 's';//按钮文字变成倒计时对应秒数
							
							//如果当秒数小于等于0时 停止计时器 且按钮文字变成重新发送 且按钮变成可用状态 倒计时的秒数也要恢复成默认秒数 即让获取验证码的按钮恢复到初始化状态只改变按钮文字
							if (self.currentTime <= 0) {
								clearInterval(interval)
								
								self.hasSend = false;
								self.text='重新发送';
								self.currentTime= 61;
								
							}
							
						}, 1000);
					}else{
						self.$Utils.showToast('发送失败', 'none', 1000)
					};
				};
				self.$apis.codeGet(postData, callback);
			},
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
					if (self.submitData.phone.trim().length != 11 || !/^1[3|4|5|6|7|8|9]\d{9}$/.test(self.submitData.phone)) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('手机格式不正确', 'none')	
						return
					};
					const callback = (user, res) => {
						console.log(res)
						self.userInfoUpdate();
					};
					self.$Utils.getAuthSetting(callback);
				
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			userInfoUpdate(phone) {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {
					phone:phone
				};
				postData.refreshToken = true;
				if(uni.getStorageSync('parent_no')){
					postData.saveAfter = [
						{
							tableName: 'Distribution',
							FuncName: 'add',
							data: {
								thirdapp_id:2,
								status:1,
								parent_no:uni.getStorageSync('parent_no'),
								child_no:uni.getStorageSync('user_info').user_no,
								level:1,
								type:1
							},
						},
					];
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('注册成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	};
</script>

<style>
@import "../../assets/style/orderNav.css";	
@import "../../assets/style/detail.css";
.orderNav .tt{width: 50%;}
.orderNav .tt.on::after{ width: 200rpx;}

.loginEdit{padding:60rpx 4% 0 4%;}
.loginEdit .item{line-height: 60rpx;box-sizing: border-box;padding-top: 40rpx;}
.loginEdit .item .ll{width: 18%;font-size: 30rpx;}
.loginEdit .item .icon{width: 44rpx; height: 44rpx;}
.loginEdit .item .rr{width: 82%;font-size: 26rpx;padding:16rpx 20rpx;border-bottom: 1px solid #eee;box-sizing: border-box;}
.loginEdit .item .rr input{width: 100%;height: 40rpx;line-height: 40rpx; display: block;font-size: 24rpx;}
.loginEdit .item .rr .placeholder{font-size: 26rpx;}
</style>

