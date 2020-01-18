<template>
	<view>
		
		
		<view class="mglr4 xqInfor">
			<view class="center pdtb15">健康知识标题标题标题</view>
			<view class="cont fs13">
				<view>管理客服电话还房贷两个号和个梵蒂冈悲愤交加鹤骨鸡肤供货方点击可供货方都拉黑干活的放假开个会过分的话看刚发的刚发的个人个人赛退热贴热推</view>
				<view>花港饭店两三个号换个卡富家大室联合国利干活附近的开个会发和公交卡方大化工干活附近的看和</view>
				<view>监管科了付赛的价格过节费考虑到加工费来电管家发了个结果翻到鬼斧神工金融机构日就是个交管理费见到过和公司就发了活过来了供货方的记录可挂号费了家里就结果回访供货方健康的两个号</view>
				<view><image src="../../static/images/health-information-img.png" mode="widthFix"></image></view>
			</view>
		</view>
		
		<view class="FxIcon"><image src="../../static/images/health-information-icon.png" mode=""></image></view>
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
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
	.FxIcon{position: fixed;top: 2%;right: 30rpx;}
	.FxIcon image{width: 90rpx;height: 90rpx;display: block;}
</style>
