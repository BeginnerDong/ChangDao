<template>
	<view>
		<view class="center pr" style="padding: 80rpx 4%;">
			<view class="flexCenter fs12">
				<view class="fs22">{{mainData.length}}</view>个
			</view>
			<!-- <view class="ewmBtn flexEnd fs12" @click="ewmShow" style="background: #C09E63; color: #fff;">
				<image class="icon" src="../../static/images/promote-icon3.png" mode=""></image>
				推广二维码
			</view> -->
		</view>
		<view class="f5H5"></view>
		
		<view class="myRowBetween myRowBetweenL70  mglr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="ll flex" style="width: 50%;">
					<view class="photo"><image :src="item.user&&item.user.mainImg&&item.user.mainImg[0]?item.user.mainImg[0].url:''" mode=""></image></view>
					<view class="fs13 ll-tit">{{item.user&&item.user.name?item.user.name:''}}</view>
				</view>
				<view class="rr color9" style="width: 50%;">{{item.create_time}}</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="ewmAlert" v-show="is_ewmShow">
			<view class="closeBtn" @click="ewmShow">×</view>
			<image class="ewm" :src="QrData.url" mode=""></image>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				spendData:[{},{},{}],
				is_show:false,
				is_ewmShow:false,
				mainData:[],
				QrData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getQrData'], self);
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
			
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getStaffToken'
				postData.qrInfo = {
					scene: wx.getStorageSync('staffInfo').user_no,
					path: 'pages/index/index',
				};
				postData.output = 'url';
				postData.ext = 'png';
				const callback = (res) => {
					if (res.solely_code == 100000) {
						self.QrData = res.info;
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
					self.$Utils.finishFunc('getQrData');
				};
				self.$apis.getQrCode(postData, callback);
			},
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					type:2,
					parent_no:uni.getStorageSync('staffInfo').user_no
				};
				postData.tokenFuncName = 'getStaffToken';
				postData.getAfter = {
					user:{
						tableName:'UserInfo',
						middleKey:'child_no',
						key:'user_no',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['name','mainImg']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.distriGet(postData, callback);
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/balance.css";
	.ll-tit{width: 78%;}
</style>
