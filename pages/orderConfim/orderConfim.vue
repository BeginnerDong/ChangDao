<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="proRow">
				<view class="item" v-for="item in mainData">
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">{{item.product&&item.product.title?item.product.title:''}}</view>
							<view class="B-price flexRowBetween">
								<view class="price">{{item.product&&item.product.title?item.product.price:''}}</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>	
			
			<view class="pdlr4 pdt15 pdb15 flexRowBetween whiteBj">
				<view>时间</view>
				<view class="fs12 color6 flexEnd">
					<hTimePicker sTime="0" cTime="24" interval="20" @changeTime="changeTime">
					<view slot="pCon" class="changeTime" style="display: flex;">{{passage1!=''?passage1:'请选择预约时间'}}</view>
					</hTimePicker>
				</view>
			</view>
			
			<view class="whiteBj servicer mgt15">
				<view class="fs13 color9">选择服务者</view>
				<view class="cont">
					<view class="lis flexRowBetween" v-for="(item,index) in staffData" :key="index">
						<view class="flex" @click="seltServicer(item.user_no)">
							<image class="seltIcon" :src="passage2==item.user_no?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image>
							<view>{{item.name}}</view>
						</view>
						<view class="flexEnd" @click="Router.navigateTo({route:{path:'/pages/staffDetail/staffDetail'}})"><image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar">
			<view class="flex mgl15 fs13">总计：<view class="price fs14">{{totalPrice}}</view></view>
			<button class="payBtn" open-type="getUserInfo"  @getuserinfo="Utils.stopMultiClick(submit)">提交订单</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="PayShow" v-show="is_PayShow">
			<view class="closebtn" @click="PayShow">×</view>
			<view class="center ftw fs22 red">￥{{totalPrice}}</view>
			<view class="flexCenter mgt10">
				<view class="countDown">请在<span class="red">19:57</span>内完成支付</view>
			</view>
			<view class="seltLis fs13">
				<view class="item flexRowBetween" @click="changePay('1')">
					<view class="flex">
						<image class="icon" src="../../static/images/the-order-icon3.png" mode=""></image>
						<view>余额支付</view>
					</view>
					<view><image class="seltIcon" :src="payCurr==1?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="changePay('2')">
					<view class="flex">
						<image class="icon" src="../../static/images/the-order-icon4.png" mode=""></image>
						<view>微信支付</view>
					</view>
					<view><image class="seltIcon" :src="payCurr==2?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image></view>
				</view>
			</view>
			<view class="submitbtn" style="margin-top:80rpx ;">
				<button class="btn" type="submint"   @click="Utils.stopMultiClick(goPay)">立即支付</button>
			</view>
		</view>
		
	</view>
</template>

<script>
	import hTimePicker from "@/components/h-timePicker/h-timePicker.vue";
	export default {
		components: { hTimePicker },
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_show:false,
				seltNum:0,
				servicerData:[{},{},{},{},{},{}],
				payCurr:1,
				is_PayShow:false,
				passage1:'',
				passage2:'',
				mainData:[],
				staffData:[],
				totalPrice:0,
				pay:{},
				isMember:false
			}
		},
		
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			self.$Utils.loadAll(['getStaffData','getUserInfoData'], self);
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				var nowTime = Date.parse(new Date())/1000;
				console.log('852369')
				const postData = {
					searchItem:{thirdapp_id:2}
				};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no		
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						if(self.userInfoData.level>0&&parseInt(self.userInfoData.deadline)>nowTime){
							self.isMember = true
						}
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
					self.countTotalPrice()
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				var member_price = 0; 
				var normal_price = 0; 
				for (var i = 0; i < self.mainData.length; i++) {
					member_price += self.mainData[i].product.member_price;
					normal_price += self.mainData[i].product.price
				};
				if(self.isMember){
					var money = parseFloat(member_price);
				}else{
					var money = parseFloat(normal_price);
				}
				self.totalPrice = parseFloat(money)
				//console.log('wxPay',wxPay)
				if (money > 0) {
					self.pay.score = {
						price: money.toFixed(2),
					};
				} else {
					  delete self.pay.score;	 
				};
				if(self.isMember){
					self.pay.other = {
						price:(parseFloat(normal_price) - parseFloat(member_price)).toFixed(2)
					}
				}
				console.log(self.pay)
			},
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				
					if(self.passage1==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请填写预约时间','none')
						return
					};
					if(self.passage2==''){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('请选择服务员工','none')
						return
					};
					var data = {
						passage1:self.passage1,
						passage2:self.passage2,
						
					}
					var orderList = [
						{product_id:self.mainData[0].product_id,count:1,type:1,data:data}
					];
					const callback = (user, res) => {
						self.addOrder(orderList)
					};
					self.$Utils.getAuthSetting(callback);
			},
			
			addOrder(orderList) {
				const self = this;	
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						
						self.orderId = res.info.id;
						//self.goPay()
						self.is_show = !self.is_show;
						self.is_PayShow = !self.is_PayShow;
					} else {		
						
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				/* var shopMoney = parseFloat(self.firstMoney) - parseFloat(self.firstMoney)*(parseFloat(self.mainData[0].product.tax)/100)
				var platformMoney = parseFloat(self.firstMoney) - parseFloat(shopMoney); */
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				/* postData.payAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							count:parseFloat(shopMoney).toFixed(2),
							thirdapp_id:2,
							status:1,
							trade_info:'首付款',
							type:2,
							account:1,
							user_no:self.mainData[0].product.user_no,
							relation_user:uni.getStorageSync('user_info').user_no
						},
					},
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							count:parseFloat(platformMoney).toFixed(2),
							thirdapp_id:2,
							status:1,
							trade_info:'平台抽佣',
							type:2,
							account:1,
							user_no:'U910872296194660',
							relation_user:self.mainData[0].product.user_no
						},
					},
				]; */
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user-orderInfor/user-orderInfor'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user-orderInfor/user-orderInfor'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			getStaffData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName ='getProjectToken';
				postData.searchItem = {
					user_type:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.staffData.push.apply(self.staffData,res.info.data);
						self.passage2 = self.staffData[0].user_no
					}
					self.$Utils.finishFunc('getStaffData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			changeTime(e){
				const self = this;
				self.passage1 = e
				console.log(e)
			},
			
			seltServicer(item){
				const self = this;
				self.passage2 = item
			},
			
			PayShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_PayShow = !self.is_PayShow;
			},
			changePay(payCurr){
				const self = this;
				if(payCurr!=self.payCurr){
					self.payCurr = payCurr
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/detail.css";
	page{background: #F5F5F5;}
	button{
		background: none;
		line-height: 1.5;
		margin-left: 0;
		margin-right: 0;
		font-size: 15px;
		border-radius: 0;
	}
	button::after{
		border: none;
	}
	.servicer{padding: 30rpx 4%;}
	.servicer .cont{max-height: 500rpx;overflow-y: auto;}
	.servicer .lis{margin-top: 30rpx;}
	.servicer .lis .seltIcon{width: 30rpx;height: 30rpx;margin-right: 20rpx;}
	.PayShow{padding: 50rpx 4% 80rpx 4%;position:fixed; left: 0;right: 0;bottom: 0;z-index: 45;background: #fff;}
	.countDown{line-height: 50rpx;background: #F5F5F5;padding: 0 30rpx;font-size: 24rpx; color: #999;border-radius: 30rpx;}
	.seltLis{margin-top: 60rpx;}
	.seltLis .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	.seltLis .item .icon{width: 40rpx;height: 40rpx; display: block;margin-right: 20rpx;}
	.seltLis .item .seltIcon{width: 36rpx;height: 36rpx;}
</style>
