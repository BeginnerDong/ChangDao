<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15">
			<view class="seachBox flexCenter color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索</view>
			</view>
			<view class="banner-box">
				<view class="banner pdtb15">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#c09e63">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
		</view>
		
		<view class="indHome flexRowBetween whiteBj">
			<view class="item"  @click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})">
				<image src="../../static/images/home-icon1.png"></image>
				<view class="tit">理疗</view>
			</view>
			<view class="item" @click="Router.redirectTo({route:{path:'/pages/product/product'}})">
				<image src="../../static/images/home-icon2.png"></image>
				<view class="tit">产品</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/health/health'}})">
				<image src="../../static/images/home-icon3.png"></image>
				<view class="tit">健康知识</view>
			</view>
			<view class="item"  @click="Router.navigateTo({route:{path:'/pages/VipInfor/VipInfor'}})">
				<image src="../../static/images/home-icon4.png"></image>
				<view class="tit">会员信息</view>
			</view>
			<view class="item" @click="Router.navigateTo({route:{path:'/pages/login/login'}})">
				<image src="../../static/images/home-icon5.png"></image>
				<view class="tit">扫一扫</view>
			</view>
		</view>
		
		<view class="mglr4 flexRowBetween productList pdtb15">
			<view class="item radius10" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
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
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">理疗</view>
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
				labelData: [
					"../../static/images/home-banner.png",
					"../../static/images/home-banner.png"
				],
				productData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
</style>
