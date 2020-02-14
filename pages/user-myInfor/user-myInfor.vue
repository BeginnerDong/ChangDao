<template>
	<view>
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween" >
				<view class="ll flex">个人信息</view>
				<view class="rr">
					<view class="userPhoto"><open-data type="userAvatarUrl"></open-data></view>
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">昵称</view>
				<view class="rr"><open-data type="userNickName"></open-data></view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">微信号</view>
				<view class="rr"><input type="text" v-model="submitData.wechat" placeholder="请输入微信号" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">手机号</view>
				<view class="rr"><input type="text" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">生日</view>
				<view class="rr">
					<picker mode="date" @change="birthdayChange">
						{{submitData.birthday!=''?submitData.birthday:'请选择生日'}}
					</picker>
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">性别</view>
				<view class="rr flexEnd" @click="seltSexShow">
					<view class="color9">{{submitData.gender==1?'男':'女'}}</view>
					<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image>
				</view>
			</view>
		</view>
			
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" open-type="getUserInfo" @getuserinfo="Utils.stopMultiClick(submit)">确定</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="seltSexShow radius10 whiteBj fs13" v-show="is_seltSexShow">
			<view class="item" @click="choose('1')">男</view>
			<view class="item" @click="choose('2')">女</view>
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
				score:'',
				wx_info:{},
				is_seltSexShow:false,
				is_show:false,
				submitData:{
					wechat:'',
					phone:'',
					birthday:'',
					gender:1
				}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			seltSexShow(){
				const self = this;
				self.is_show = true;
				self.is_seltSexShow =true;
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('完善成功', 'none', 1000)
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.name = self.mainData.name;
						if(self.submitData.gender!=0){
							self.submitData.gender = self.mainData.gender;
						};
						self.submitData.phone = self.mainData.phone;
						self.submitData.wechat = self.mainData.wechat;
						self.submitData.birthday = self.mainData.birthday;
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			birthdayChange(e){
				const self = this;
				self.submitData.birthday = e.detail.value;
				
				console.log(self.submitData);
			},
			
			choose(item){
				const self = this;
				self.submitData.gender = item
			},
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
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

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.userPhoto{width: 80rpx;height: 80rpx;border-radius: 50%;overflow: hidden;}
	.userPhoto image{width: 100%;height: 100%;border-radius: 50%;display: block;}
	
	.seltSexShow{width: 80%;position: fixed; top: 50%;left: 50%;z-index: 50;transform: translate(-50%,-50%);}
	.seltSexShow .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	.seltSexShow .item:last-child{border-bottom: 0;}
</style>
