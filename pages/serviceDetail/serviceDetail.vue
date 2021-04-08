<template>
	<view>
		
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="mglr4 pdtb15 fs14">
			<view class="tit mgb10">{{mainData.title?mainData.title:''}}</view>
			<view class="flexRowBetween">
				<view class="flex">
					<view class="price fs16 ftw">{{mainData.price?mainData.price:''}}</view>
					<view class="flex VipPrice fs10">
						<view>会员价</view>
						<view class="mny">{{mainData.member_price?mainData.member_price:''}}</view>
					</view>
					<view class="flex VipPrice fs10">
						<view>超级会员价</view>
						<view class="mny">{{mainData.super_price?mainData.super_price:''}}</view>
					</view>
				</view>
				<button class="fs12 color6 flexEnd" v-if="!staffShare" open-type="share" id="userShare">
					<image class="shareIcon mgr5" src="../../static/images/details-icon.png" mode=""></image>分享
				</button>
				<button class="fs12 color6 flexEnd" v-if="staffShare" open-type="share" id="staffShare">
					<image class="shareIcon mgr5" src="../../static/images/details-icon.png" mode=""></image>分享
				</button>
			</view>
			
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15 flexRowBetween" @click="phoneCall">
			<view class="fs13">电话：{{mainData.phone?mainData.phone:''}}</view>
			<view class="flexEnd"><image style="width: 34rpx;height: 34rpx;" src="../../static/images/details-icon1.png" mode=""></image></view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4">
			<view class="orderNav pdt5 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="change('1')">详情</view>
				<view class="tt" :class="curr==2?'on':''" @click="change('2')">评论</view>
			</view>
			<view class="">
				<view class="xqInfor pdtb15 fs13" v-show="curr==1">
					<view class="cont">
						<view class="content ql-editor" style="padding:0;"
						v-html="mainData.content">
						</view>
					</view>
				</view>
				<view class="pingjia" v-show="curr==2">
					<view class="item" v-if="messageData.length>0" v-for="(item,index) in messageData" :key="index">
						<view class="flexRowBetween pdb10">
							<view class="flex">
								<view class="photo mgr10">
									<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
								</view>
								<view class="fs13">{{item.title}}</view>
							</view>
							<view class="flexEnd color6 fs12">{{item.create_time}}</view>
						</view>
						<view class="fs13">{{item.description}}</view>
					</view>
					<view class="item" v-if="messageData.length==0">
						
						<view class="fs13" style="text-align: center;">暂无评价~</view>
					</view>
					<view class="pdtb15 flexCenter" v-if="messageData.length>2">
						<view class="fs13 color6 flexCenter" @click="Router.navigateTo({route:{path:'/pages/evaluate/evaluate?id='+mainData.id}})">查看更多<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15">
			<view class="flexRowBetween pdb10">
				<view class="ftw fs15">全部商品</view>
				<view class="fs12 color9 flexEnd" 
				@click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})">全部<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="flexRowBetween productList">
				<view class="item radius10" v-for="(item,index) in productData" :data-id="item.id"
				:key="index" @click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					
					<view class="infor">
						<view class="tit avoidOverflow">{{item.title}}</view>
						<view class="price fs16 ftw tit">{{item.price}}</view>
						<view class="flex  fs10">
							<view>会员价</view>
							<view class="mny">{{item.member_price}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar submitbtn" style="padding: 40rpx 4%;">
				<button class="btn" type="button" 
				@click="Utils.stopMultiClick(goBuy)">去结算</button>
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
				curr:1,
				pingjiaData:[{},{}],
				productData:[],
				mainData:{},
				messageData:[],
				isShare:false,
				staffShare:false,
				pay:{},
				isMember:false,
				ratio:0,
				realMoney:0,
				isSuperMember:false
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			if(uni.getStorageSync('staffInfo')){
				self.staffShare = true
			};
			if(options.user_no){
				self.isShare = true;
				uni.setStorageSync('parent_no',options.user_no)
			};
			console.log('options',options)
			self.$Utils.loadAll(['getMainData','getProductData','getMessageData','getDistriData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
			self.getUserInfoData()
		},
		
		onShareAppMessage(ops) {
			console.log(ops)
			const self = this;
			if (ops.target.id === 'staffShare') {
				
				return {
					title: '常道-'+self.mainData.title,
					path: '/pages/serviceDetail/serviceDetail?id='+self.mainData.id+'&user_no='+uni.getStorageSync('staffInfo').user_no, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
			}else{
				return {
					title: '常道-'+self.mainData.title,
					path: '/pages/serviceDetail/serviceDetail?id='+self.mainData.id+'&user_no='+uni.getStorageSync('user_info').user_no, //点击分享的图片进到哪一个页面
					imageUrl:self.mainData&&self.mainData.mainImg&&self.mainData.mainImg[0]&&self.mainData.mainImg[0].url?self.mainData.mainImg[0].url:'',
					success: function(res) {
						// 转发成功
						console.log("转发成功:" + JSON.stringify(res));
					},
					fail: function(res) {
						// 转发失败
						console.log("转发失败:" + JSON.stringify(res));
					}
				}
				console.log(ops.target)
			}
		},
		
		methods: {
			
			message(){
				const self = this;
				 //self.goPay()
				wx.requestSubscribeMessage({
				  tmplIds: ['0hcW79pWe-rRGN0tb5ov_TiqreKFyTR2ZosT030_sYY','Gs4a4LMFYrcMQGPfimitXa6JoGEKoPe28O_Fs2UfMuQ'],
				  success (res) { 
					  console.log(res)
					  if(res){
						  self.goPay()
					  }
				  },
				  fail(err) {    //失败
				  					
				  	self.goPay()
				  }
				})
			},
			
			getDistriData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					child_no:uni.getStorageSync('user_info').user_no
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					user:{
						tableName:'UserInfo',
						middleKey:'parent_no',
						key:'user_no',
						condition:'=',
						searchItem:{
							status:1
						},
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.distriData =  res.info.data[0]
					};
					if(self.distriData&&self.distriData.user&&self.distriData.user[0]&&self.distriData.user[0].user_type==1){
						self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.staff)/100;
					}else if(self.distriData&&self.distriData.user&&self.distriData.user[0]&&self.distriData.user[0].user_type==0){
						if(self.distriData.user[0].level==0){
							self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.user)/100;
						}else if(self.distriData.user[0].level==1){
							self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.member_one)/100;
						}else if(self.distriData.user[0].level==2){
							self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.member_two)/100;
						}else if(self.distriData.user[0].level==3){
							self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.member_three)/100;
						}else if(self.distriData.user[0].level==4){
							self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.member_four)/100;
						}else if(self.distriData.user[0].level==5){
							self.ratio = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.member_super)/100;
						}
					};
					console.log(self.ratio)
					self.$Utils.finishFunc('getDistriData');
				};
				self.$apis.distriGet(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if(!self.isShare){
							if(self.userInfoData.phone==''){
								self.Router.navigateTo({route:{path:'/pages/register/register'}})
							}
						}
						if(self.userInfoData.level>0&&self.userInfoData.level<5){
							self.isMember = true
						}else if(self.userInfoData.level==5){
							self.isSuperMember = true
						}
						self.countTotalPrice()
					}
					console.log('self.userInfoData', self.userInfoData)
					//self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				var member_price = 0; 
				var normal_price = 0; 
				var super_price = 0; 
				
					member_price += self.mainData.member_price;
					normal_price += self.mainData.price;
					super_price += self.mainData.super_price;
				
				if(self.isMember){
					var money = parseFloat(member_price);
				}else if(self.isSuperMember){
					var money = parseFloat(super_price);
				}else{
					var money = parseFloat(normal_price);
				}
				self.totalPrice = parseFloat(money).toFixed(2)
				//console.log('wxPay',wxPay)
				
				self.pay.score = {
					price: money.toFixed(2),
				};
				self.realMoney = self.pay.score.price;
				
				if(self.isMember){
					self.pay.other = {
						price:(parseFloat(normal_price) - parseFloat(member_price)).toFixed(2)
					}
				}else if(self.isSuperMember){
					self.pay.other = {
						price:(parseFloat(normal_price) - parseFloat(super_price)).toFixed(2)
					}
				}
				console.log(self.pay)
				
			},
			
			phoneCall(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber:self.mainData.phone
				})
			},
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				if(self.userInfoData.phone==''){
					uni.showModal({
						title: '提示',
						content: '您还未注册，清先进行注册',
						success: function(res) {
							if (res.confirm) {
								self.Router.navigateTo({route:{path:'/pages/register/register'}})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
					uni.setStorageSync('canClick',true);
					return
				};
				var data = {
					pay_price:self.realMoney
				};
				self.orderList = [];
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData,data:data},
				);
				uni.setStorageSync('payPro', self.orderList);
				if(self.mainData.type==1){
					wx.requestSubscribeMessage({
					  tmplIds: ['0hcW79pWe-rRGN0tb5ov_TiqreKFyTR2ZosT030_sYY','Gs4a4LMFYrcMQGPfimitXa6JoGEKoPe28O_Fs2UfMuQ'],
					  success (res) { 
						  console.log(res)
						  if(res){
							  self.addOrder()
						  }
					  },
					  fail(err) {    //失败
					  					
					  	self.addOrder()
					  }
					})
					//self.Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})
				}else if(self.mainData.type==2){
					self.Router.navigateTo({route:{path:'/pages/product-orderConfim/product-orderConfim'}})
				}
				uni.setStorageSync('canClick',true);
			},
			
			addOrder(orderList) {
				const self = this;	
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(self.orderList);
				postData.data = {};
				postData.tokenFuncName = 'getProjectToken';
				if(!wx.getStorageSync('user_info')||wx.getStorageSync('user_info').headImgUrl==''||!wx.getStorageSync('user_info').headImgUrl){
				  postData.refreshToken = true;
				};
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						
						self.orderId = res.info.id;
						self.goPay()
						
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
				
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				if(self.ratio>0){
					postData.payAfter = [
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								count:(parseFloat(self.totalPrice)*self.ratio).toFixed(2),
								thirdapp_id:2,
								status:1,
								trade_info:'推广佣金',
								type:2,
								account:1,
								behavior:2,
								user_no:self.distriData.parent_no,
								relation_user:uni.getStorageSync('user_info').user_no
							},
						},
					];
				}
				
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
							title: '余额不足',
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
			
			change(curr) {
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getMessageData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 3
				};
				postData.searchItem = {
					thirdapp_id: 2,
					relation_id :self.id,
					type:1,
					user_type:0
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.messageData.push.apply(self.messageData, res.info.data);
					}
					console.log('23',res.info.total)
					self.totalMessage = res.info.total;
					console.log('self.messageData', self.messageData)
					self.$Utils.finishFunc('getMessageData');
				};
				self.$apis.messageGet(postData, callback);
			},
			
			getProductData() {
				const self = this;
				const postData = {};
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
				};
				postData.searchItem = {
					thirdapp_id:2,
					id:['not in',self.id]
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData.push.apply(self.productData,res.info.data)
					}
					self.$Utils.finishFunc('getProductData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	@import "../../assets/style/VipPrice.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/pingjia.css";
	@import "../../assets/style/product.css";
	page{padding-bottom: 150rpx;}
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
</style>
