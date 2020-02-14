<template>
	<view>
		
		<view class="pdlr4 pswdCont">
			<view class="item">
				<view class="tit">原始密码</view>
				<view class="input">
					<input type="text" value="" placeholder="请输入原始密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item">
				<view class="tit">新密码</view>
				<view class="input">
					<input type="password" value="" placeholder="请输入新密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item">
				<view class="tit">再次输入新密码</view>
				<view class="input">
					<input type="password" value="" placeholder="请确认新密码" placeholder-class="placeholder">
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="submit">确认</button>
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
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {


		},
	};
</script>

<style>
	page{padding-bottom: 130rpx;}
	.pswdCont .item{margin-top: 50rpx;}
	.pswdCont .tit{margin-bottom: 20rpx;}
	.pswdCont .item input{width: 100%;height:80rpx;line-height: 40rpx;padding: 20rpx;box-sizing: border-box;font-size: 26rpx;background: #F5F5F5;border-radius: 10rpx;}
</style>
