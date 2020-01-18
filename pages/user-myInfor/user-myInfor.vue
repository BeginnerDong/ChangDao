<template>
	<view>
		
		<view class="myRowBetween fs13 mglr4">
			<view class="item flexRowBetween" >
				<view class="ll flex">个人信息</view>
				<view class="rr">
					<view class="userPhoto"><image src="../../static/images/about-img.png" mode=""></image></view>
				</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">昵称</view>
				<view class="rr">哆啦A梦</view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">微信号</view>
				<view class="rr"><input type="text" value="" placeholder="请输入微信号" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">手机号</view>
				<view class="rr"><input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">生日</view>
				<view class="rr"><input type="text" value="" placeholder="请输入生日" placeholder-class="placeholder"></view>
			</view>
			<view class="item flexRowBetween" >
				<view class="ll flex">性别</view>
				<view class="rr flexEnd" @click="seltSexShow">
					<view class="color9">请选择</view>
					<image class="arrowR" src="../../static/images/the-order-icon2.png" mode=""></image>
				</view>
			</view>
		</view>
			
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button">确定</button>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="seltSexShow radius10 whiteBj fs13" v-show="is_seltSexShow">
			<view class="item" @click="seltSexShow">男</view>
			<view class="item" @click="seltSexShow">女</view>
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
				is_seltSexShow:false,
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			seltSexShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_seltSexShow = !self.is_seltSexShow
			}

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.userPhoto{width: 80rpx;height: 80rpx;border-radius: 50%;overflow: hidden;}
	.userPhoto image{width: 100%;height: 100%;border-radius: 50%;display: block;}
	
	.seltSexShow{width: 80%;position: fixed; top: 50%;left: 50%;z-index: 50;transform: translate(-50%,-50%);}
	.seltSexShow .item{padding: 30rpx 4%;border-bottom: 1px solid #eee;}
	.seltSexShow .item:last-child{border-bottom: 0;}
</style>
