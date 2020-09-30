<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="活动" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"><text>×</text></view>
			</view>
			<view class="fs15 pubColor">搜索</view>
		</view>
		<view class="mglr4 flexRowBetween productList mgt15">
			<view class="item radius10" v-for="(item,index) in mainData" :data-id="item.id"
			:key="index" @click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">{{item.title}}</view>
					<view class="price fs16 ftw tit">{{item.price}}</view>
					<view class="flex  fs10 tit">
						<view>会员价</view>
						<view class="mny">{{item.member_price}}</view>
					</view>
					<view class="flex  fs10">
						<view>超级会员价</view>
						<view class="mny">{{item.super_price}}</view>
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
				wx_info:{},
				is_show:false,
				productData:[{},{},{},{},{}],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.keywords){
				self.keywords = options.keywords;
			}
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
			// self.$Utils.loadAll(['getMainData'], self);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id:2,
					title:['like',['%'+self.keywords+'%']]
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/product.css";
	
	page{padding-bottom: 40rpx;background: #F5F5F5;}
	.seachbox{padding: 30rpx 4% 20rpx 4%;box-sizing: border-box;}
	.seachbox .rr{width: 85%;background:#F5F5F5;border-radius: 40rpx;overflow: hidden;}
	.seachbox .rr .input{width:78%;}
	.seachbox .rr .input input{width: 100%;line-height: 60rpx;padding: 0 20rpx 0 0px;box-sizing: border-box;display: block;border: none;font-size: 26rpx;}
	.seachbox .rr .seachBtn{width: 12%;height: 60rpx; display: block;background: url(../../static/images/home-icon.png) no-repeat center center/30rpx 30rpx;border: none;}
	.seachbox .delt{width: 10%; height: 26rpx;}
	.seachbox .delt text{width: 28rpx;height: 28rpx;border-radius: 50%;background: #cfcfcf; color: #fff; line-height: 26rpx;text-align: center;}
	
</style>

