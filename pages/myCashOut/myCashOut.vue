<template>
	<view>
		
		<view class="tixianBox radius10">
			
			<view class="flexRowBetween bankmsg addBtn" v-if="userInfoData.card_no!=''" >
				<view>到账银行卡</view>
				<view class="lft fs12">
					{{userInfoData.bank}}
					<view class="color6 num">({{userInfoData.card_no}})</view>
				</view>
				<view @click="goBank">
					<image class="arrow arrowR" src="../../static/images/the-order-icon2.png" mode=""></image>
				</view>
			</view>
			
			<view class="flexRowBetween bankmsg addBtn" >
				<view>到账银行卡</view>
				<view class="flexCenter" @click="goBank">
					<text class="color9 fs13">去添加</text>
					<image class="arrow arrowR" src="../../static/images/the-order-icon2.png" mode=""></image>
				</view>
			</view>
		
			
		
			<view class="editMoney">
				<view class="tit">提现金额</view>
				<view class="input">
					<input type="number" v-model="submitData.count" placeholder="请输入提现金额" placeholder-class="placeholder">
				</view>
				<view class="flexRowBetween fs12 color6">
					<view>可提现金额{{userInfoData.balance}}元</view>
					<view class="color2" @click="allCount">全部提现</view>
				</view>
				<view class="submitbtn" style="margin-top: 100rpx;">
					<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">提现</button>
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
				Utils:this.$Utils,
				mainData:{},
				userInfoData:{},
				submitData:{
					count:''
				},
				isStaff:false
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.type){
				self.isStaff = true
			}
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			goBank(){
				const self = this;
				if(self.isStaff){
					self.Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg?type=staff'}})
				}else{
					self.Router.navigateTo({route:{path:'/pages/myBankMsg/myBankMsg'}})
				}
			},
			
			allCount(){
				const self = this;
				self.submitData.count = self.userInfoData.balance
			},
			
			submit() {
				const self = this;
				wx.requestSubscribeMessage({
				  tmplIds: ['erhS7WH5XbfMelKP5vRFtU4UHzEsDHAc3k0kE-pG1m0','Gs4a4LMFYrcMQGPfimitXa6JoGEKoPe28O_Fs2UfMuQ'],
				  success (res) { 
					  console.log(res)
					  if(res){
						 uni.setStorageSync('canClick', false);
						 if(!self.userInfoData.card_no){
						 	uni.setStorageSync('canClick', true);
						 	self.$Utils.showToast('请先绑定银行卡', 'none');
						 	return
						 };
						 const pass = self.$Utils.checkComplete(self.submitData);
						 console.log('self.submitData',self.submitData)
						 if (pass) {
						 	if(parseFloat(self.submitData.count)>parseFloat(self.userInfoData.balance)){
						 		uni.setStorageSync('canClick', true);
						 		self.$Utils.showToast('余额不足', 'none');
						 		return
						 	};
						 	if(parseFloat(self.submitData.count)<=0){
						 		uni.setStorageSync('canClick', true);
						 		self.$Utils.showToast('请输入正确的金额', 'none');
						 		return
						 	};
						 		self.flowLogAdd();
						 } else {
						 	uni.setStorageSync('canClick', true);
						 	self.$Utils.showToast('请输入提现金额', 'none')
						 };
					  }
				  }
				})
			},
			
			flowLogAdd() {
				const self = this;
				const postData = {};
				if(self.isStaff){
					postData.tokenFuncName = 'getStaffToken';
				}else{
					postData.tokenFuncName = 'getProjectToken';
				}
				postData.data = {
					count:-self.submitData.count,
					thirdapp_id:2,
					status:1,
					trade_info:'提现',
					type:2,
					account:1,
					withdraw:1,
					withdraw_status:0,
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('提交成功', 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 800)
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.flowLogAdd(postData, callback);
			},
			
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				if(self.isStaff){
					postData.tokenFuncName = 'getStaffToken';
					postData.searchItem.user_no = uni.getStorageSync('staffToken').user_no
				}else{
					postData.tokenFuncName = 'getProjectToken';
					postData.searchItem.user_no = uni.getStorageSync('user_info').user_no
				}
				
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0]
						self.userInfoData.card_no = self.userInfoData.card_no.substr(self.userInfoData.card_no.length-4)
					} else {
						self.$Utils.showToast(res.msg, 'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
			
				};
				self.$apis.userInfoGet(postData, callback);
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
