<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white pr">
			<view class="money">{{userInfoData.balance}}</view>
			<view class="fs13 pdt10">余额(元)</view>
			<view class="txBtn pubColor whiteBj"  @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut?type=staff'}})">提现</view>
		</view>
		<view class="userBox2 flexRowBetween fs12">
			<view class="child flexColumn">
				<view class="fs15 ftw">{{userInfoData.service?userInfoData.service.count:''}}</view>
				<view class="color6">服务佣金</view>
			</view>
			<view class="child flexColumn">
				<view class="fs15 ftw">{{userInfoData.spread?userInfoData.spread.count:''}}</view>
				<view class="color6">推广佣金</view>
			</view>
			<view class="child flexColumn">
				<view class="fs15 ftw">{{userInfoData.team?userInfoData.team.count:''}}</view>
				<view class="color6">团队佣金</view>
			</view>
		</view>
		
		<view class="orderNav flexRowBetween borderB1 color6">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">服务佣金</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">推广佣金</view>
			<view class="tt" :class="num==3?'on':''" @click="change('3')">团队佣金</view>
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
			<view class="myRowBetween mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="fs13">{{item.trade_info}}</view>
						<view class="fs12 color6">{{item.create_time}}</view>
					</view>
					<view class="rr red">{{item.count}}</view>
				</view>
			</view>
			<view class="myRowBetween myRowBetweenL70  mglr4" v-show="num==3">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll flex" style="width: 50%;">
						<view class="photo"><image :src="item.user&&item.user.mainImg&&item.user.mainImg[0]?item.user.mainImg[0].url:''" mode=""></image></view>
						<view class="fs13">{{item.user&&item.user.name?item.user.name:''}}</view>
					</view>
					<view class="rr red" style="width: 50%;">{{item.count}}</view>
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
				
				num:1,
				mainData:[],
				userInfoData:{},
				searchItem:{
					type:2,
					status:1,
					behavior:1
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getMainData'], self);
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
			
			getUserInfoData() {
				const self = this;
				console.log('852369')
				const postData = {
					searchItem:{}
				};
				postData.tokenFuncName = 'getStaffToken'
				postData.searchItem.user_no = uni.getStorageSync('staffInfo').user_no		
				postData.getAfter = {
					
					service: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							behavior:1,
							type:2
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,
						      behavior:1,
						      type:2
						    }
						  ]
						},
					},
					spread: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							behavior:2,
							type:2
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,
						      behavior:2,
						      type:2
						    }
						  ]
						},
					},
					team: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							behavior:2,
							type:3
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,
						      behavior:3,
						      type:2
						    }
						  ]
						},
					},
				};
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
				postData.tokenFuncName = 'getStaffToken';
				postData.getAfter = {
					user:{
						tableName:'UserInfo',
						middleKey:'relation_user',
						key:'user_no',
						condition:'=',
						searchItem:{
							status:1
						},
						info:['mainImg','name']
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
			
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num;
					self.searchItem.behavior = self.num;
					self.getMainData(true)
				}
			}
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/balance.css";
	
</style>
