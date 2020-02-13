<template>
	<view>
		
		<view class="mglr4 healthList">
			<view class="item flexRowBetween radius10" v-for="(item,index) in mainData" :data-id="item.id"
			:key="index" @click="Router.navigateTo({route:{path:'/pages/healthDetail/healthDetail?id='+$event.currentTarget.dataset.id}})">
				<view class="ll"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
				<view class="rr">
					<view class="tit avoidOverflow">{{item.title}}</view>
					<view class="text fs12 color6 avoidOverflow2">{{item.description}}</view>
					
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
				healthData:[{},{},{},{}],
				mainData:[],
				searchItem:{
					thirdapp_id:2
				}
			}
		},
		
		onLoad() {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			console.log(2323)
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
				console.log(2323)
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 5
					}
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getBefore = {
					article:{
						tableName:'Label',
						middleKey:'menu_id',
						key:'id',
						searchItem:{
							title: ['in', ['健康知识']],
						},
						condition:'in'
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.articleGet(postData, callback);
			},
		}
	};
</script>

<style>
	
	page{padding-bottom:60rpx;}
	.healthList .item{padding: 30rpx 0;align-items: flex-start;border-bottom: 1px solid #eee;}
	.healthList .ll{width: 260rpx;height: 200rpx;overflow: hidden;border-radius: 10rpx;}
	.healthList .ll image{width: 100%;height: 100%; display: block;}
	.healthList .rr{width: 60%;height: 200rpx;padding: 10rpx 0;box-sizing: border-box;position: relative;}
	.healthList .rr .text{line-height: 36rpx;margin-top: 80rpx;position: absolute;left: 0;right: 0;bottom: 16rpx;}
</style>
