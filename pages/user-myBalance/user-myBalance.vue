<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white">
			<view class="money">{{userInfoData.score}}</view>
			<view class="fs13 pdt10">余额(元)</view>
		</view>
		<view class="userBox2 boxShaow flexRowBetween radius10 whiteBj">
			<view class="">产生利息</view>
			<view class="red">{{userInfoData.interest?userInfoData.interest.count:''}}元</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4">
				<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
					<view class="ll">
						<view class="fs26">{{item.trade_info}}</view>
						<view class="fs12 color6">{{item.create_time}}</view>
					</view>
					<view class="rr red">{{item.count}}</view>
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
				num:1,
				spendData:[{},{},{}],
				mainData:[],
				userInfoData:{},
				searchItem:{
					type:3,
					thirdapp_id:2,
					
				},
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData','getUserInfoData'], self);
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
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem.user_no = uni.getStorageSync('user_info').user_no		
				postData.getAfter = {
					
					interest: {
						tableName: 'FlowLog',
						searchItem: {
							status:1,
							behavior:4,
						},
						middleKey: 'user_no',
						key: 'user_no',
						condition: 'in',
						compute:{
						  count:[
						    'sum',
						    'count',
						    {
						      status:1,behavior:4
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
				postData.tokenFuncName = 'getProjectToken';
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.flowLogGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/balance.css";
	
</style>
