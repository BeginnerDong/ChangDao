<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"  style="padding-bottom: 30rpx;"> 
			<view class="flex rr" style="width: 85%;">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text"  v-model="keywords" placeholder="输入关键字搜索" placeholder-class="placeholder" />
				</view>
				<view class="delt flex"><text>×</text></view>
			</view>
			<view class="fs15 pubColor" @click="search">搜索</view>
		</view>
		
		<view class="pdlr4 borderB1">
			<view class="fs15 pdt20 pdb10 ftw">热门搜索</view>
			<view class="hotLabel flex center">
				<view class="item" v-for="(item,index) in mainData" @click="clickSearch(item.word)" :key="index">{{item.word}}</view>
			</view>
			<view class="fs15 pdt20 pdb10 ftw">搜索记录</view>
			<view class="historyDate  center fs13 flex fs12">
				<view class="item" v-for="(item,index) in historyDate" @click="clickSearch(item)"  :key="index">{{item}}</view>
			</view>
		</view>
		
		<view class="fx-deltBtn flexCenter color6" @click="popupShow">
			<image class="icon" src="../../static/images/address-icon1.png" mode=""></image>
			<text>清除搜索记录</text>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">确定清空搜索记录吗？</view>
			<view class="flex tip-button">
				<view class="item"  @click="popupShow">取消</view>
				<view class="item pubColor" @click="clearHistory()">确定</view>
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
				mainData:[],
				historyDate:[],
				is_popupShow:false,
				keywords:''
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log(uni.getStorageSync('historyDate'))
			if(uni.getStorageSync('historyDate')){
				self.historyDate = uni.getStorageSync('historyDate')
			};
			console.log(self.historyDate)
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		
		methods: {
			
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			},
			
			clickSearch(item){
				const self = this;
				self.Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct?keywords='+item}});
			},
			
			search(){
				const self = this;
				if(self.keywords!=''){
					self.Router.navigateTo({route:{path:'/pages/seachProduct/seachProduct?keywords='+self.keywords}});
					self.historyDate.push(self.keywords);
					uni.setStorageSync('historyDate',self.historyDate);
					self.keywords = '';
				}else{
					return
				}
			},
			
			clearHistory(){
				const self = this;
				self.historyDate = [];
				uni.removeStorageSync('historyDate');
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
			},
			
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
					thirdapp_id:2
				};
				postData.order = {
					count:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData,res.info.data)
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.hotSearchGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	/* @import "../../assets/style/seach.css"; */
	
	page{padding-bottom: 110rpx;}
	
	.seachbox{padding: 30rpx 4% 20rpx 4%;box-sizing: border-box;}
	.seachbox .Gps{max-width: 140rpx;}
	.seachbox .Licon{width: 20rpx; height: 26rpx; display: block;}
	.seachbox .rr{width: 85%;background:#F5F5F5;border-radius: 40rpx;overflow: hidden;}
	.seachbox .rr .input{width:78%;}
	.seachbox .rr .input input{width: 100%;line-height: 60rpx;padding: 0 20rpx 0 0px;box-sizing: border-box;display: block;border: none;font-size: 26rpx;}
	.seachbox .rr .seachBtn{width: 12%;height: 60rpx; display: block;background: url(../../static/images/home-icon.png) no-repeat center center/30rpx 30rpx;border: none;}
	.seachbox .delt{width: 10%; height: 26rpx;}
	.seachbox .delt text{width: 28rpx;height: 28rpx;border-radius: 50%;background: #cfcfcf; color: #fff; line-height: 26rpx;text-align: center;}
	
	.hotLabel{flex-wrap: wrap;}
	.hotLabel .item{width: 150rpx;height: 60rpx; line-height: 60rpx;background: #f0f0f0;margin:0 20rpx 20rpx 0; border-radius: 30rpx;text-align: center;}
	.hotLabel .item:nth-of-type(4n){margin-right: 0;}
	
	.historyDate .item{margin: 0 40rpx 30rpx 0;}
	.fx-deltBtn{width: 350rpx;height: 80rpx;line-height: 80rpx; border-radius: 40rpx;border:1px solid #a4a4a4; position: fixed;bottom: 70rpx;left: 50%;transform: translateX(-50%);}
	.fx-deltBtn .icon{width:36rpx; height: 36rpx;display: block;margin-right: 10rpx;}
	
	
	/* 删除弹框 */
	.popupShow{ width: 80%;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%); z-index: 50;box-sizing: border-box;}
	.tip-button .item{width: 50%;box-sizing: border-box; line-height: 100rpx;}
	.tip-button .item:first-child{border-right:1px solid #eee;}
</style>

