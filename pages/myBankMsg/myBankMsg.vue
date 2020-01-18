<template>
	<view class="editLine pdlr4">
		<view class="item flexRowBetween">
			<view class="ll">持卡人：</view>
			<view class="rr">
				<input type="text" value="张丹" placeholder="请输入持卡人的姓名"/>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="ll">手机号：</view>
			<view class="rr">
				<input type="number" value="15623565689" maxlength="11" placeholder="请输入您的银行卡绑定的手机号"/>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="ll">开户行：</view>
			<view class="rr">
				<input type="text" value="中国银行" placeholder="请输入您的银行卡的开户行"/>
			</view>
		</view>
		<view class="item flexRowBetween">
			<view class="ll">银行卡号：</view>
			<view class="rr">
				<input type="number" value="6215525245127853" placeholder="请输入您的银行卡号"/>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;" >
			<button class="btn" type="submit" @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">确定</button>
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

				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');

				};
				self.$apis.orderGet(postData, callback);

			},

		},
	};
</script>
<style>
	@import "../../assets/style/editInfor.css";
	.editLine .item .ll{color: #666;}
	.editLine .item input{color: #222;}
</style>

 
