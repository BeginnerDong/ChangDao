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
				productData:[{},{},{},{},{}]
			}
		},
		
		onLoad(options) {
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

