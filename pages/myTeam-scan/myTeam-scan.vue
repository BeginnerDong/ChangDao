<template>
	<view>
		<view class="mglr4 mgt15">
			<view class="proRow">
				<view class="item">
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/the-order-img.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">按摩精油</view>
							<view class="B-price flexRowBetween">
								<view class="price">889.00</view>
								<view class="fs13">×1</view>
							</view>
						</view>
					</view>
				</view>
			</view>	
			
			<view class="pdlr4 pdt15 pdb15 flexRowBetween whiteBj">
				<view>时间</view>
				<view class="fs13 color6">2020.01.18 15:20</view>
			</view>
			
			<view class="whiteBj servicer mgt15">
				<view class="fs13 color9">选择服务者</view>
				<view class="cont">
					<view class="lis flex" @click="seltServicer(index)" v-for="(item,index) in servicerData" :key="index">
						<image class="seltIcon" :src="seltNum==index?'../../static/images/the-order-icon.png':'../../static/images/the-order-icon1.png'" mode=""></image>
						<view>张月</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 160rpx;">
			<button class="btn" type="button">确认</button>
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
				is_show:false,
				seltNum:0,
				servicerData:[{},{},{},{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			seltServicer(index){
				const self = this;
				self.seltNum = index
			}
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;}
	.servicer{padding: 30rpx 4%;}
	.servicer .cont{max-height: 400rpx;overflow-y: auto;}
	.servicer .lis{margin-top: 30rpx;}
	.servicer .lis .seltIcon{width: 30rpx;height: 30rpx;margin-right: 20rpx;}
</style>
