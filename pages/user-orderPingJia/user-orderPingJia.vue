<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">已使用</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">按摩精油</view>
							<view class="B-price flexRowBetween">
								<view class="price">56.00</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="whiteBj pdlr4 pdt15 pdb15">
				<view>评价</view>
				<view class="mgt10 f5bj">
					<textarea value="" placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top:200rpx;">
			<button class="btn" type="button">确定</button>
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
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;}
	textarea{color: #222;}
</style>
