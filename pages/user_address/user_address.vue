<template>
	<view>
			<view class="myaddress-lis" v-for="(item,index) in mainData" :key="index">
				<view class="name">{{item.name}}<view class="numb">{{item.phone}}</view></view>
				<view class="adrs">{{item.city+item.detail}}</view>
				<view class="seltBox">
					<view class="L"  :data-id="item.id" @click="updateAddress($event.currentTarget.dataset.id)">
						<image class="icon" :src="item.isdefault==1?'../../static/images/address-icon2.png':'../../static/images/address-icon3.png'"  mode=""></image>
						默认地址
					</view>
					<view class="R fs12 color9 flexEnd">
						<view class="child flexEnd mgr20" :data-id="item.id" @click="Router.navigateTo({route:{path:'/pages/user_addressAdd/user_addressAdd?id='+$event.currentTarget.dataset.id}})"><image src="../../static/images/address-icon.png" mode=""></image>编辑</view>
						<view class="child flexEnd" :data-id="item.id" @click="deleteAddress($event.currentTarget.dataset.id)"><image src="../../static/images/address-icon1.png" mode=""></image>删除</view>
					</view>
				</view>
			</view>
			
			<view class="submitbtn" style="margin-top: 200rpx;">
				<button class="btn" type="button"  @click="Router.navigateTo({route:{path:'/pages/user_addressAdd/user_addressAdd'}})">添加新地址</button>
			</view>
			
			<!-- 删除弹框 -->
			<view class="seltAlert" v-if="is_show">
				<view class="infor radius10 center">
					<view class="closebtn"  @click="deltAlert">×</view>
					<view class="tit font16" style="padding-bottom: 60rpx;">确认是否删除这个地址</view>
					<view class="btnB flexRowBetween">
						<view>是</view>
						<view  class="on" @click="deltAlert">否</view>
					</view>
				</view>
			</view>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				mainData: [],
				Router:this.$Router,
				choosedIndex: -1
			}
		},

		onShow(options) {
			const self = this;
			self.mainData = [];
			var res = self.$Token.getProjectToken(function() {
				self.$Utils.loadAll(['getMainData'], self)
			});
			if (res) {
				self.$Utils.loadAll(['getMainData'], self)
			};
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

			choose(index) {
				const self = this;
				self.choosedIndex = index;
				uni.setStorageSync('choosedAddressData', self.mainData[index]);
				console.log('choosedIndex', self.choosedIndex);
			},

			getMainData() {
				const self = this;

				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.addressGet(postData, callback);
			},





			deleteAddress(id) {
				const self = this;
				uni.showModal({
					title: '提示',
					content: '确认是否删除这个地址',
					success: function(res) {
						if (res.confirm) {
							const postData = {};
							postData.searchItem = {};
							postData.searchItem.id = id;
							postData.tokenFuncName = 'getProjectToken';
							const callback = (res) => {
								if (res) {
									self.mainData = [];
									self.getMainData();
								}
							};
							self.$apis.addressDelete(postData, callback)
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},


			updateAddress(id) {
				const self = this;
				const postData = {};

				postData.tokenFuncName = 'getProjectToken';

				postData.searchItem = {};
				postData.searchItem.id = id;
				postData.data = {
					isdefault: 1
				}
				const callback = (res) => {
					if (res) {
						self.mainData = [];
						self.getMainData();
					}
				};
				self.$apis.addressUpdate(postData, callback);
			},




		}
	}
</script>

<style>
	@import "../../assets/style/seltAlert.css";
	page{ background: #f5f5f5;}
	.myaddress-lis{padding:30rpx 4%;margin-bottom: 20rpx;background: #fff; margin-bottom: 30rpx;}
	.myaddress-lis .name{ font-size: 28rpx; color: #222;}
	.myaddress-lis .numb{margin-left: 30rpx; width: 300rpx; display: inline-block;}
	.myaddress-lis .adrs{ font-size: 26rpx;color: #999; line-height: 40rpx;padding: 10rpx 0;}
	.seltBox{ display: flex;justify-content: space-between; align-items: center;padding-top: 10rpx;}
	.seltBox .L{ width: 30%; display: flex; align-items: center; font-size: 24rpx; color: #999;}
	.seltBox .L .icon{ width: 30rpx; height: 30rpx; display: inline-block; margin-right: 10rpx;}
	.seltBox .R{ width: 70%;}
	.seltBox .R image{  width:30rpx; height: 30rpx; display:block; margin-right: 8rpx;}

</style>
