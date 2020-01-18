<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img.png" mode=""></image>
		</view>
		<view class="mglr4 pdtb15 fs14">
			<view class="tit mgb10">推拿按摩手法、推拿按摩时要注意这些事情</view>
			<view class="flexRowBetween">
				<view class="flex">
					<view class="price fs16 ftw">889</view>
					<view class="flex VipPrice fs10">
						<view>会员价</view>
						<view class="mny">48</view>
					</view>
				</view>
				<view class="fs12 color6 flexEnd"><image class="shareIcon mgr5" src="../../static/images/details-icon.png" mode=""></image>分享</view>
			</view>
			
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15 flexRowBetween">
			<view class="fs13">电话：15623562356</view>
			<view class="flexEnd"><image style="width: 34rpx;height: 34rpx;" src="../../static/images/details-icon1.png" mode=""></image></view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4">
			<view class="orderNav pdt5 flexRowBetween">
				<view class="tt" :class="curr==1?'on':''" @click="change('1')">详情</view>
				<view class="tt" :class="curr==2?'on':''" @click="change('2')">评论</view>
			</view>
			<view class="">
				<view class="xqInfor pdtb15 fs13" v-show="curr==1">
					<view class="cont">
						<view>管理客服电话还房贷两个号和个梵蒂冈悲愤交加鹤骨鸡肤供货方点击可供货方都拉黑干活的放假开个会过分的话看</view>
						<view>花港饭店两三个号换个卡富家大室联合国利干活附近的开个会发和公交卡方大化工干活附近的看和</view>
						<view><image class="w" src="../../static/images/details-img1.png" mode="widthFix"></image></view>
					</view>
				</view>
				<view class="pingjia" v-show="curr==2">
					<view class="item" v-for="(item,index) in pingjiaData" :key="index">
						<view class="flexRowBetween pdb10">
							<view class="flex">
								<view class="photo mgr10">
									<image src="../../static/images/details-img2.png" mode=""></image>
								</view>
								<view class="fs13">快乐的猫</view>
							</view>
							<view class="flexEnd color6 fs12">2020.01.17</view>
						</view>
						<view class="fs13">好的浓香型白酒用语是,酒体清亮透明,入口绵甜,落口爽净,回味悠长,值得信赖。</view>
					</view>
					<view class="pdtb15 flexCenter">
						<view class="fs13 color6 flexCenter" @click="Router.navigateTo({route:{path:'/pages/evaluate/evaluate'}})">查看更多<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
					</view>
				</view>
			</view>
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15">
			<view class="flexRowBetween pdb10">
				<view class="ftw fs15">全部商品</view>
				<view class="fs12 color9 flexEnd">全部<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="flexRowBetween productList">
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
		
		<view class="xqbotomBar submitbtn"style="padding: 40rpx 4%;">
				<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})" >去结算</button>
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
				curr:1,
				pingjiaData:[{},{}],
				productData:[{},{},{},{}]
			}
		},
		
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(curr) {
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
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
	@import "../../assets/style/detail.css";
	@import "../../assets/style/VipPrice.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/pingjia.css";
	@import "../../assets/style/product.css";
	page{padding-bottom: 150rpx;}
	
</style>
