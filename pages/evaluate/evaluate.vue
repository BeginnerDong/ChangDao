<template>
	<view>
		
		<view class="mglr4 pingjia">
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
				pingjiaData:[{},{},{},{}]
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
	@import "../../assets/style/pingjia.css";
	page{padding-bottom: 60rpx;}
	
</style>
