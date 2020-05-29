<template>
	<view>
		
		<view class="couponList mglr4">
			<view class="item mgt15 whiteBj boxShaow">
				<view class="topName">
					<view class="bj"><image src="../../static/images/vouchersl-img.png" mode=""></image></view>
					<view class="cont flexRowBetween">
						<view class="icon"><image src="../../static/images/vouchersl-icon.png" mode=""></image></view>
						<view class="title white">
							<view class="fs16">{{mainData.title}}</view>
							<view class="fs12 mgt5">电子券套餐</view>
						</view>
					</view>
				</view>
				<view class="B-infor flexRowBetween">
					<view class="fs13 color6">使用时间：永久有效</view>
					<!-- <view class="ewm" ><image src="../../static/images/my-order-img.png" mode=""></image></view> -->
				</view>
			</view>
		</view>
		
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="$Utils.stopMultiClick(orderUpdate)">确认核销</button>
		</view>
		
		
		<!-- <view class="black-bj" v-show="is_show"></view>
		<view class="ewmAlert" v-show="is_ewmShow">
			<view class="closeBtn" @click="ewmShow">×</view>
			<image class="ewm" src="../../static/images/my-order-img.png" mode=""></image>
		</view> -->
		
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
				num:1,
				is_show:false,
				couponData:2,
				is_ewmShow:false,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getStaffToken';
				postData.data = {
					transport_status:2,
				};
				postData.searchItem = {
					id:self.id,
					user_type:0
				};
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
			
			getMainData() {
				const self = this;
				const postData = {
					searchItem:{
						id:self.id,
					}
				};
				postData.tokenFuncName = 'getStaffToken'
				//postData.searchItem.user_no = uni.getStorageSync('staffInfo').user_no		
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
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
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
					
				}
			},
			
			ewmShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow
			}
		}
	};
</script>

<style>
	page{background: #F5F5F5;}
	image{width: 100%;height: 100%;display: block;}
	
	.couponList .item{width: 100%;height: 294rpx;box-sizing: border-box;border-radius: 10rpx;overflow: hidden;}
	.topName{position: relative;height: 160rpx;padding: 0 40rpx;}
	.topName .bj{position: absolute;left: 0;top: 0;right: 0;bottom: 0;}
	.topName .cont{position: relative;z-index: 2;height: 148rpx;}
	.topName .cont .icon{width: 90rpx;height: 90rpx;}
	.topName .cont .title{text-align: right;}
	.couponList .item .ewm{width: 80rpx;height: 80rpx;}
	.B-infor{height: 134rpx;padding: 0 40rpx;}
	
	.ewmAlert{position: fixed;top: 40%;left: 50%;transform: translateX(-50%);z-index: 45;}
	.ewmAlert .ewm{width: 360rpx;height: 360rpx;display: block;}
	
</style>
