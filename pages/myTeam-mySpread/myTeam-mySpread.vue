<template>
	<view>
		<view class="center pr" style="padding: 80rpx 4%;">
			<view class="flexCenter fs12">
				<view class="fs22">10</view>个
			</view>
			<view class="ewmBtn flexEnd fs12" @click="ewmShow" style="background: #C09E63; color: #fff;">
				<image class="icon" src="../../static/images/promote-icon3.png" mode=""></image>
				推广二维码
			</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="myRowBetween myRowBetweenL70  mglr4">
			<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
				<view class="ll flex">
					<view class="photo"><image src="../../static/images/promote-img.png" mode=""></image></view>
					<view class="fs13 ll-tit">圣诞节狂欢夜</view>
				</view>
				<view class="rr color9">2020.01.17</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="ewmAlert" v-show="is_ewmShow">
			<view class="closeBtn" @click="ewmShow">×</view>
			<image class="ewm" src="../../static/images/promote-img1.png" mode=""></image>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				score:'',
				wx_info:{},
				spendData:[{},{},{}],
				is_show:false,
				is_ewmShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			ewmShow(){
				const self = this;
				self.is_show = !self.is_show;
				self.is_ewmShow = !self.is_ewmShow
			}
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/balance.css";
	.ll-tit{width: 78%;}
</style>
