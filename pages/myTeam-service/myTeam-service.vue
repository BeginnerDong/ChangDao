<template>
	<view>
		
		<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15">
			<view class="tt" :class="serviceNum==1?'on':''" @click="serviceChange('1')">提供服务</view>
			<view class="tt" :class="serviceNum==2?'on':''" @click="serviceChange('2')">预约待服务</view>
		</view>
		<view class="mglr4">
			<view class="proRow" v-if="mainData.length>0">
				<view class="item" v-for="item in mainData">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color6">时间：{{item.create_time}}</view>
						<view class="red" v-if="item.transport_status==2">已核销</view>
						<view class="red" v-if="item.transport_status==0">待核销</view>
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
					<view class="dashedB1 mgt15" v-if="item.transport_status==2"></view>
					<view class="adviseF5 fs12"  v-if="item.transport_status==2">
						<span class="ftw">健康管理意见：</span>{{item.passage3}}
					</view>
				</view>
			</view>
			<view v-else style="text-align: center;">暂无数据~</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
				
				serviceNum:1,
				mainData:[],
				searchItem:{
					pay_status:1,
					type:1,
					thirdapp_id:2,
					user_type:0,
					transport_status:2
				}
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
				postData.tokenFuncName = 'getStaffToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.searchItem.passage2 = uni.getStorageSync('staffInfo').user_no;
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
			
			
			serviceChange(serviceNum){
				const self = this;
				if(serviceNum!= self.serviceNum){
					self.serviceNum = serviceNum;
					if(self.serviceNum==1){
						self.searchItem.transport_status = 2;
					}else if(self.serviceNum==2){
						self.searchItem.transport_status = 0;
					}
					self.getMainData(true)
				}
			},
			
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
