<template>
	<view>
		
		<view class="mglr4 flexRowBetween productList mgt15">
			<view class="item radius10" v-for="(item,index) in productData" :key="index" @click="Router.navigateTo({route:{path:'/pages/productDetail/productDetail'}})">
				<view class="pic"><image src="../../static/images/product-img.png" mode=""></image></view>
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
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">理疗</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/product/product'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">产品</view>
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
	
	page{padding-bottom: 120rpx;background: #F5F5F5;}
</style>
