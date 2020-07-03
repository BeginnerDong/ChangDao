<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15">
			<view class="seachBox flexCenter color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})" v-if="sliderData.url&&sliderData.url!='1'">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索</view>
			</view>
			<view class="banner-box">
				<view class="banner pdtb15">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#c09e63">
						<block v-for="(item,index) in sliderData.mainImg" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item.url" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		<view v-if="sliderData.url&&sliderData.url!='1'">
			<view class="indHome flexRowBetween whiteBj">
				<!-- <view class="item"  @click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})">
					<image src="../../static/images/home-icon1.png"></image>
					<view class="tit">理疗</view>
				</view>
				<view class="item" @click="Router.redirectTo({route:{path:'/pages/product/product'}})">
					<image src="../../static/images/home-icon2.png"></image>
					<view class="tit">产品</view>
				</view> -->
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/health/health'}})">
					<image src="../../static/images/home-icon3.png"></image>
					<view class="tit">健康知识</view>
				</view>
				<view class="item"  @click="Router.navigateTo({route:{path:'/pages/VipInfor/VipInfor'}})">
					<image src="../../static/images/home-icon4.png"></image>
					<view class="tit">加入会员</view>
				</view>
				<view class="item" @click="Router.navigateTo({route:{path:'/pages/myTeam-login/myTeam-login?type=index'}})">
					<image src="../../static/images/home-icon5.png"></image>
					<view class="tit">扫一扫</view>
				</view>
			</view>
			
			<view class="mglr4 flexRowBetween productList pdtb15">
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
			
			<!--底部tab键-->
			<view class="navbar">
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
					<view class="nav_img">
						<image src="../../static/images/nabar1-a.png" />
					</view>
					<view class="text this-text">首页</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})" >
					<view class="nav_img">
						<image src="../../static/images/nabar2.png" />
					</view>
					<view class="text">{{sliderData.url&&sliderData.url!='1'?'理疗':'菜单'}}</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/product/product'}})" >
					<view class="nav_img">
						<image src="../../static/images/nabar3.png" />
					</view>
					<view class="text">产品</view>
				</view>
				<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
					<view class="nav_img">
						<image src="../../static/images/nabar4.png" />
					</view>
					<view class="text">我的</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键 end-->
		<view class="mglr4 healthList"  v-else>
			<view class="item flexRowBetween radius10" style="margin-top: 20rpx;background-color: #fff;" v-for="(item,index) in artData" :data-id="item.id"
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
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				productData:[{},{},{},{},{},{}],
				sliderData:{},
				mainData:[],
				searchItem:{
					thirdapp_id:2
				},
				artData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.scene){
				var scene = decodeURIComponent(options.scene);
				uni.setStorageSync('parent_no',scene)
			};
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getSliderData','getMainData','getUserInfoData','getShopData','getArtData'], self);
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
			
			getArtData(isNew) {
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
						self.artData.push.apply(self.artData,res.info.data)
					}
					self.$Utils.finishFunc('getArtData');
				};
				self.$apis.articleGet(postData, callback);
			},
			
			getShopData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					title:'商家信息',
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						uni.setStorageSync('shopInfo',res.info.data[0])
					}
					self.$Utils.finishFunc('getShopData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				/* ss */
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getSliderData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					title:'首页轮播',
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.sliderData = res.info.data[0]
					}
					self.$Utils.finishFunc('getSliderData');
				};
				self.$apis.labelGet(postData, callback);
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
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
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
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/product.css";
	@import "../../assets/style/VipPrice.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.seachBox{border-radius: 30rpx;background: #f5f5f5;padding:10rpx 80rpx 10rpx 30rpx;position: relative;box-sizing: border-box;}
	.seachBox input{width: 100%;min-height: auto;line-height: 40rpx;height: 40rpx;display: block;font-size: 26rpx;}
	.seachBox .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.seachBox .rr{width: 60rpx;height: 40rpx;position: absolute; right: 10rpx;top: 50%;transform: translateY(-50%);}	
	.seachBox .btn{width:100%;height: 100%;background: url(../../static/images/home-icon.png) no-repeat center center /30rpx 30rpx;}	
	
	.swiper-box {height: 300rpx;border-radius: 10rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item{width: 100%;}
	.swiper-box image {width: 100%;height: 100%;}
	
	/* 分类导航 */
	.indHome{ flex-wrap:wrap;}
	.indHome .item{width: 20%;text-align: center;padding-bottom: 15px; font-size: 26rpx; color: #222;display: flex; flex-direction: column;}
	.indHome .item image{width:90rpx;height:90rpx; margin: 0 auto 20rpx auto; }
	
	.healthList .item{padding: 30rpx 0;align-items: flex-start;border-bottom: 1px solid #eee;}
	.healthList .ll{width: 260rpx;height: 200rpx;overflow: hidden;border-radius: 10rpx;}
	.healthList .ll image{width: 100%;height: 100%; display: block;}
	.healthList .rr{width: 60%;height: 200rpx;padding: 10rpx 0;box-sizing: border-box;position: relative;}
	.healthList .rr .text{line-height: 36rpx;margin-top: 20rpx;}
</style>
