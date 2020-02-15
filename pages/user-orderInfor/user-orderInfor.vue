<template>
	<view>
		<view class="tooling_indNav">
			<view class="list">
				<view class="item" :class="curr==1?'on':''" @click="changeCurr('1')">服务订单</view>
				<view class="item" :class="curr==2?'on':''" @click="changeCurr('2')">产品订单</view>
			</view>
		</view>
		
		<view >
			<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15" v-if="searchItem.type==1">
				<view class="tt" :class="serviceNum==1?'on':''" @click="serviceChange('1')">全部</view>
				<view class="tt" :class="serviceNum==2?'on':''" @click="serviceChange('2')">待使用</view>
				<view class="tt" :class="serviceNum==3?'on':''" @click="serviceChange('3')">已使用</view>
				<view class="tt" :class="serviceNum==4?'on':''" @click="serviceChange('4')">已评价</view>
			</view>
			<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15" v-if="searchItem.type==2">
				<view class="tt" :class="proNum==1?'on':''" @click="proChange('1')">全部</view>
				<view class="tt" :class="proNum==2?'on':''" @click="proChange('2')">待发货</view>
				<view class="tt" :class="proNum==3?'on':''" @click="proChange('3')">待收货</view>
				<view class="tt" :class="proNum==4?'on':''" @click="proChange('4')">已收货</view>
				<view class="tt" :class="proNum==5?'on':''" @click="proChange('5')">已评价</view>
			</view>
			<view class="mglr4">
				<view class="proRow">
					<view class="item"  v-for="(item,index) in mainData">
						<view class="fs12 flexRowBetween mgb10" v-if="item.type==1">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==0&&item.isremark==0">待使用</view>
							<view class="red" v-if="item.transport_status==2&&item.isremark==0">已使用</view>
							<view class="red" v-if="item.isremark==1">已评价</view>
						</view>
						<view class="fs12 flexRowBetween mgb10" v-if="item.type==2">
							<view class="color9">交易时间：{{item.create_time}}</view>
							<view class="red" v-if="item.transport_status==0&&item.isremark==0">待发货</view>
							<view class="red" v-if="item.transport_status==1&&item.isremark==0">待收货</view>
							<view class="red" v-if="item.transport_status==2&&item.isremark==0">已收货</view>
							<view class="red" v-if="item.isremark==1">已评价</view>
						</view>
						<view class="flexRowBetween">
							<view class="pic">
								<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="avoidOverflow2 fs13">{{item.title}}</view>
								<view class="B-price flexRowBetween">
									<view class="price">{{item.price}}</view>
									<view class="fs13">×1</view>
								</view>
							</view>
						</view>
						<view class="dashedB1 mgt15"></view>
						<view v-if="item.type==1">
							<view class="LRbet fs12" v-if="item.transport_status==0">
								<!-- <view class="line pdt15 flexRowBetween">
									<view class="ll">在线客服</view>
									<view class="rr flexEnd"><image class="kefuIcon" src="../../static/images/my-order-icon.png" mode=""></image></view>
								</view>
								<view class="line pdt15 flexRowBetween">
									<view class="ll">地址</view>
									<view class="rr flexEnd">陕西省西安市长安区韦曲街道</view>
								</view> -->
								<view class="line pdt15 flexRowBetween">
									<view class="ll">联系电话</view>
									<view class="rr flexEnd">
										<view>{{item.orderItem&&item.orderItem[0]&&
							item.orderItem[0].snap_product?item.orderItem[0].snap_product.phone:''}}</view>
										<image class="phoneIcon mgl10" src="../../static/images/my-order-icon1.png" mode=""></image></view>
								</view>
								<view class="line pdt15 flexRowBetween">
									<view class="ll">地址</view>
									<view class="rr flexEnd">陕西省西安市长安区韦曲街道</view>
								</view>
								<view class="line pdt15 flexRowBetween">
									<view class="ll"></view>
									<view class="rr flexEnd">
										<image class="sEwm" :src="item.qrcode" mode=""></image>
									</view>
								</view>
								
							</view>
							<view class="LRbet fs12" v-if="item.transport_status==2&&item.isremark==0">
								<view class="adviseF5 fs12">
									<span class="ftw">健康管理意见：</span>{{item.passage3}}
								</view>
								<view class="underBtn flexEnd mgt15">
									<view class="Bbtn" 
									:data-id="item.id"
									@click="Router.navigateTo({route:{path:'/pages/user-orderPingJia/user-orderPingJia?id='+$event.currentTarget.dataset.id}})">去评价</view>
								</view>
							</view>
							<view class="LRbet fs12" v-if="item.isremark==1">
								<view class="adviseF5 fs12">
									<span class="ftw">健康管理意见：</span>{{item.passage3}}
								</view>
								<view class="adviseF5 fs12 mgt15">
									<span class="ftw">评价：</span>{{item.remark&&item.remark.description?item.remark.description:''}}
								</view>
							</view>
						</view>
						<view v-if="item.type==2">
							<view class="underBtn flexEnd mgt15">
								<!-- <view class="Bbtn">去支付</view> -->
								<view class="Bbtn" v-if="item.transport_status==1">确认收货</view>
								<view class="Bbtn" v-if="item.transport_status==2&&item.isremark==0" :data-id="item.id" 
						@click="Router.navigateTo({route:{path:'/pages/user-orderPingJia/user-orderPingJia?id='+$event.currentTarget.dataset.id}})">去评价</view>
							</view>
							<view class="adviseF5 fs12 mgt15" v-if="item.isremark==1">
								<span class="ftw">评价：</span>{{item.remark&&item.remark.description?item.remark.description:''}}
							</view>
						</view>
					</view>
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
				score:'',
				wx_info:{},
				curr:1,
				serviceNum:1,
				is_show:false,
				proNum:1,
				searchItem:{
					type:1,
					pay_status:1
				},
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
					remark:{
						tableName:'Message',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'=',
						info:['description']
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr;
					self.searchItem.type=self.curr;
					self.serviceNum = 1;
					self.proNum = 1;
					delete self.searchItem.transport_status;
					delete self.searchItem.isremark;
					self.getMainData(true)
				}
			},
			
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum;
					if(self.serviceNum==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
					}else if(self.serviceNum==2){
						self.searchItem.transport_status = 0;
						delete self.searchItem.isremark;
					}else if(self.serviceNum==3){
						self.searchItem.transport_status = 2;
						self.searchItem.isremark = 0
						
					}else if(self.serviceNum==4){
						self.searchItem.isremark = 1
					}
					self.getMainData(true)
				}
			},
			
			proChange(proNum){
				const self = this;
				if(proNum!= self.proNum){
					self.proNum = proNum
					if(self.proNum==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
					}else if(self.proNum==2){
						self.searchItem.transport_status = 0;
						delete self.searchItem.isremark;
					}else if(self.proNum==3){
						self.searchItem.transport_status = 1;
						delete self.searchItem.isremark;
						
					}else if(self.proNum==4){
						self.searchItem.transport_status = 2;
						self.searchItem.isremark = 0
					}else if(self.proNum==5){
						self.searchItem.transport_status = 2;
						self.searchItem.isremark = 1
					}
					self.getMainData(true)
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;}
	.kefuIcon{width: 34rpx;height: 36rpx; display: block;}
	.phoneIcon{width: 27rpx;height: 34rpx; display: block;}
	.sEwm{width: 100rpx;height: 100rpx; display: block;}
	.LRbet .line{padding-top: 30rpx;}
	.LRbet .ll{width: 20%;}
	.LRbet .rr{width: 80%;}
	.adviseF5{padding: 30rpx;background: #f5f5f5;}
</style>
