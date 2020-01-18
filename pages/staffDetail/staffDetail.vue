<template>
	<view>
		
		<view class="mglr4 pdt20 pdb15">
			<view class="ftw fs14 mgb10">简介</view>
			<view class="xqInfor">
				<view class="cont fs12">
					<view>管理客服电话还房贷两个号和个梵蒂冈悲愤交加鹤骨鸡肤供货方点击可供货方都拉黑干活的放假开个会过分的话看刚发的刚发的个人个人赛退热贴热推</view>
					<view>花港饭店两三个号换个卡富家大室联合国利干活附近的开个会发和公交卡方大化工干活附近的看和</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="mglr4 pdt20 pdb15">
			<view class="ftw fs14 mgb10">相册</view>
			<view class="photoBox flexRowBetween">
				<view class="item" v-for="(item,index) in photoData" :key="index"><image src="../../static/images/jianjie-img.png" mode=""></image></view>
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
				photoData:[{},{},{},{}]
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
	@import "../../assets/style/detail.css";
	page{padding-bottom:60rpx;}
	
	.photoBox{flex-wrap: wrap;align-items: flex-start;}
	.photoBox .item{width: 330rpx;height: 220rpx;overflow: hidden;margin-bottom: 30rpx;}
	.photoBox .item image{width: 100%;height: 100%; display: block;}
</style>
