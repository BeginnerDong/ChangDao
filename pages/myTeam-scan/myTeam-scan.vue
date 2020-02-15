<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="proRow">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
							mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">{{mainData.title}}</view>
							<view class="B-price flexRowBetween">
								<view class="price">{{mainData.price}}</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>	
			
			<view class="pdlr4 pdt15 pdb15 flexRowBetween whiteBj">
				<view>时间</view>
				<view class="fs13 color6">{{mainData.passage1}}</view>
			</view>
			
			<view class="whiteBj servicer mgt15">
				<view class="fs13 color9">选择服务者</view>
				<view class="cont">
					<view class="lis flex" @click="seltServicer(item.user_no)" v-for="(item,index) in staffData" :key="index">
						<image class="seltIcon" :src="passage2==item.user_no?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image>
						<view>{{item.name}}</view>
					</view>
					
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button" @click="$Utils.stopMultiClick(orderUpdate)">确认</button>
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
				staffData:[],
				passage2:''
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getStaffData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				var ratio = parseFloat(uni.getStorageSync('staffInfo').thirdApp.custom_rule.service)/100;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getStaffToken';
				postData.data = {
					transport_status:2,
					passage2:self.passage2
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
				postData.saveAfter = [
					{
						tableName: 'FlowLog',
						FuncName: 'add',
						data: {
							count:(parseFloat(self.mainData.price)*ratio).toFixed(2),
							thirdapp_id:2,
							status:1,
							trade_info:'服务佣金',
							type:2,
							account:1,
							behavior:1,
							order_no:self.mainData.order_no,
							user_no:self.passage2,
							relation_user:self.mainData.user_no
						},
					},
				];
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			getStaffData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName ='getStaffToken';
				postData.searchItem = {
					user_type:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.staffData.push.apply(self.staffData,res.info.data);
						
					}
					self.$Utils.finishFunc('getStaffData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			seltServicer(item){
				const self = this;
				self.passage2 = item
			},
			
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{
						id:self.id,
					}
				};
				postData.tokenFuncName = 'getStaffToken'
				//postData.searchItem.user_no = uni.getStorageSync('staffInfo').user_no		
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1,
							user_type:0
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.passage2 = self.mainData.passage2
					} else {
						self.$Utils.showToast(res.msg, 'none');
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;}
	.servicer{padding: 30rpx 4%;}
	.servicer .cont{max-height: 400rpx;overflow-y: auto;}
	.servicer .lis{margin-top: 30rpx;}
	.servicer .lis .seltIcon{width: 30rpx;height: 30rpx;margin-right: 20rpx;}
</style>
