<template>
	<view class="editLine pdlr4">
		<view class="item flexRowBetween">
			<view class="ll">持卡人：</view>
			<view class="rr">
				<input type="text" v-model="submitData.card_name" placeholder="请输入持卡人的姓名"/>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="ll">手机号：</view>
			<view class="rr">
				<input type="number" v-model="submitData.card_phone" maxlength="11" placeholder="请输入您的银行卡绑定的手机号"/>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="ll">开户行：</view>
			<view class="rr">
				<input type="text" v-model="submitData.bank" placeholder="请输入您的银行卡的开户行"/>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="ll">银行卡号：</view>
			<view class="rr">
				<input type="number" v-model="submitData.card_no" placeholder="请输入您的银行卡号"/>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;" >
			<button class="btn" type="submit" @click="Utils.stopMultiClick(submit)">确定</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				index:-1,
				labelData:[],
				Utils:this.$Utils,
				submitData:{
					card_name:'',
					bank:'',
					card_card:'',
					card_phone:'',
					
				},
				
			}
		},
		
		onShow(options) {
			const self = this;
			console.log(23243)
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			
			/* bindPickerChange(e) {	
				const self=this;
				console.log('picker发送选择改变，携带值为', e.target.value)
				self.index = e.target.value;
				self.submitData.bank=self.labelData[self.index].id
			}, */
			
			/* getLabelData() {
				const self = this;	
				const postData = {};	
				postData.searchItem = {
					thirdapp_id:2,	
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['=', ['银行类型']],
						},
						middleKey: 'parentid',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					create_time:'asc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData.push.apply(self.labelData, res.info.data);
					}
					self.getMainData()
					console.log('self.labelData',self.labelData)
					
				};
				self.$apis.labelGet(postData, callback);
			}, */
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				if (pass) {	
					self.userInfoUpdate();	
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			
			userInfoUpdate() {
				const self = this;
				var newObject = self.$Utils.cloneForm(self.submitData);
				delete newObject.smsCode;
				const postData = {};
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(newObject);
				postData.smsAuth = {
					phone:self.submitData.bank_phone,						
					code:self.submitData.smsCode
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('修改成功', 'none', 1000)
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
				postData.tokenFuncName = 'getThirdToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('thirdInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.bank_name = res.info.data[0].bank_name;
						self.submitData.bank_card = res.info.data[0].bank_card;
						self.submitData.bank_phone = res.info.data[0].bank_phone;
						self.submitData.bank = res.info.data[0].bank;
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		},
	};
</script>	
<style>
	@import "../../assets/style/editInfor.css";
	.editLine .item .ll{color: #666;}
	.editLine .item input{color: #222;}
</style>

 
