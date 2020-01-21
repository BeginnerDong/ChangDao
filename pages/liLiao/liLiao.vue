<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15 pdb15 borderB1">
			<view class="seachBox flexCenter color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索</view>
			</view>
		</view>
		<view class="pdtb15 whiteBj">
			<scroll-view id="tab-bar" class="uni-swiper-tab" scroll-x :scroll-left="scrollLeft">
				<view v-for="(tab,index) in tabBars" :key="index" class="swiper-tab-list" :class="tabIndex==index? 'on' : ''" @click="tapTab(index)">{{tab}}</view>
			</scroll-view>
		</view>
		<view class="mglr4 flexRowBetween productList mgt15">
			<view class="item radius10" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail'}})">
				<view class="pic"><image src="../../static/images/home-img.png" mode=""></image></view>
				<view class="infor">
					<view class="tit avoidOverflow">王氏按摩服务</view>
					<view class="flex">
						<view class="price fs16 ftw">56</view>
						<view class="flex VipPrice fs10">
							<view>会员价</view>
							<view class="mny">48</view>
						</view>
					</view>
					
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">理疗</view>
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
		<!--底部tab键 end-->
		
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
				tabIndex: 0,
				tabBars: ['理疗','中药','按摩','理疗','中药','按摩','理疗','中药','按摩'],
				productData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			tapTab(index) { 
				//点击tab-bar
				const self = this;
				self.tabIndex = index
			},
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	@import "../../assets/style/product.css";
	@import "../../assets/style/VipPrice.css";
	
	page{padding-bottom: 120rpx;background: #F5F5F5;}
	.seachBox{border-radius: 30rpx;background: #f5f5f5;padding:10rpx 80rpx 10rpx 30rpx;position: relative;box-sizing: border-box;}
	.seachBox input{width: 100%;min-height: auto;line-height: 40rpx;height: 40rpx;display: block;font-size: 26rpx;}
	.seachBox .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.seachBox .rr{width: 60rpx;height: 40rpx;position: absolute; right: 10rpx;top: 50%;transform: translateY(-50%);}	
	.seachBox .btn{width:100%;height: 100%;background: url(../../static/images/home-icon.png) no-repeat center center /30rpx 30rpx;}
	.uni-swiper-tab{white-space: nowrap;}
	.swiper-tab-list{display: inline-block;margin: 0 30rpx;}
	.swiper-tab-list.on{ color: #C09E63;}
</style>
