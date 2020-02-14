<template>
	<view>
		
		<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="mglr4 pdtb15 fs14">
			<view class="tit mgb10">{{mainData.title}}</view>
			<view class="flexRowBetween">
				<view class="flex">
					<view class="price fs16 ftw">{{mainData.price}}</view>
					<view class="flex VipPrice fs10">
						<view>会员价</view>
						<view class="mny">{{mainData.member_price}}</view>
					</view>
				</view>
				<view class="fs12 color6 flexEnd"><image class="shareIcon mgr5" src="../../static/images/details-icon.png" mode=""></image>分享</view>
			</view>
			
		</view>
		<view class="f5H5"></view>
		<view class="mglr4 pdtb15 flexRowBetween">
			<view class="fs13">电话：{{mainData.phone}}</view>
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
						<view class="content ql-editor" style="padding:0;"
						v-html="mainData.content">
						</view>
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
				<view class="fs12 color9 flexEnd" 
				@click="Router.redirectTo({route:{path:'/pages/liLiao/liLiao'}})">全部<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image></view>
			</view>
			<view class="flexRowBetween productList">
				<view class="item radius10" v-for="(item,index) in productData" :data-id="item.id"
				:key="index" @click="Router.navigateTo({route:{path:'/pages/serviceDetail/serviceDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="tit avoidOverflow">{{item.title}}</view>
						<view class="flex">
							<view class="price fs16 ftw">{{item.price}}</view>
							<view class="flex VipPrice fs10">
								<view>会员价</view>
								<view class="mny">{{item.member_price}}</view>
							</view>
						</view>
						
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar submitbtn" style="padding: 40rpx 4%;">
				<button class="btn" type="button" 
				@click="Utils.stopMultiClick(goBuy)">去结算</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				curr:1,
				pingjiaData:[{},{}],
				productData:[],
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getProductData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro')
		},
		
		
		methods: {
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData},
				);
				uni.setStorageSync('payPro', self.orderList);
				if(self.mainData.type==1){
					self.Router.navigateTo({route:{path:'/pages/orderConfim/orderConfim'}})
				}else if(self.mainData.type==2){
					self.Router.navigateTo({route:{path:'/pages/product-orderConfim/product-orderConfim'}})
				}
				uni.setStorageSync('canClick',true);
			},
			
			change(curr) {
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			getProductData() {
				const self = this;
				const postData = {};
				postData.paginate = {
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 4
				};
				postData.searchItem = {
					thirdapp_id:2,
					id:['not in',self.id]
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.productData.push.apply(self.productData,res.info.data)
					}
					self.$Utils.finishFunc('getProductData');
				};
				self.$apis.productGet(postData, callback);
			},
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
