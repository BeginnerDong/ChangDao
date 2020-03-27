<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white pr">
			<view class="money">{{userInfoData.balance}}</view>
			<view class="fs13 pdt10">余额(元)</view>
			<view class="txBtn pubColor whiteBj"  @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
			<view class="ewmBtn flexEnd fs12" @click="ewmShow">
				<image class="icon" src="../../static/images/promote-icon.png" mode=""></image>
				推广二维码
			</view>
		</view>
		<view class="orderNav flexRowBetween borderB1 color6">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">佣金明细</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">推广客户</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4" v-show="num==1">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="fs13">{{item.trade_info}}</view>
						<view class="fs12 color6">{{item.create_time}}</view>
					</view>
					<view class="rr red">{{item.count}}</view>
				</view>
			</view>
			<view class="myRowBetween myRowBetweenL70  mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll flex" style="width:50%">
						<view class="photo"><image :src="item.user&&item.user.headImgUrl?item.user.headImgUrl:''" mode=""></image></view>
						<view class="fs13">{{item.user&&item.user.nickname?item.user.nickname:''}}</view>
					</view>
					<view class="rr color9"  style="width:50%">{{item.create_time}}</view>
				</view>
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
				showView: false,
				score:'',
				wx_info:{},
				num:1,
				spendData:[{},{},{}],
				is_show:false,
				is_ewmShow:false,
				mainData:[],
				userInfoData:{},
				searchItem:{
					type:2,
					thirdapp_id:2,
					
				},
				QrData:{}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData','getQrData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				if(self.num==1){
					self.getMainData()
				}else if(self.num==2){
					self.getDistriData()
				}
			};
		},
		
		methods: {
			
			
			getQrData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.qrInfo = {
					scene: wx.getStorageSync('user_info').user_no,
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
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
					if(self.num==1){
						self.getMainData(true)
					}else if(self.num==2){
						self.getDistriData(true)
					}
				}
			},
			
			ewmShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow
			},
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no		
				
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getProjectToken';
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
			getDistriData(isNew) {
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
					type:1,
					parent_no:uni.getStorageSync('user_info').user_no
				};
				postData.tokenFuncName = 'getProjectToken';
				postData.getAfter = {
					user:{
						tableName:'User',
						middleKey:'child_no',
						key:'user_no',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['headImgUrl','nickname']
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
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/balance.css";
	
</style>
