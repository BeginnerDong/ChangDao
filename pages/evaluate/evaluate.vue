<template>
	<view>
		
		<view class="mglr4 pingjia">
			<view class="item" v-for="(item,index) in mainData" :key="index">
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
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false,
				pingjiaData:[{},{},{},{}],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
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
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
					relation_id :self.id,
					type:1,
					user_type:0
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('23',res.info.total)
					self.totalMessage = res.info.total;
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.messageGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/pingjia.css";
	page{padding-bottom: 60rpx;}
	
</style>
