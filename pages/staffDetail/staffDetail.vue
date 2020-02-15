<template>
	<view>
		
		<view class="mglr4 pdt20 pdb15">
			<view class="ftw fs14 mgb10">简介</view>
			<view class="xqInfor">
				<view class="cont fs12">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
				</view>
			</view>
		</view>
		<view class="f5H10"></view>
		
		<view class="mglr4 pdt20 pdb15">
			<view class="ftw fs14 mgb10">相册</view>
			<view class="photoBox flexRowBetween">
				<view class="item" v-for="(item,index) in mainData.mainImg" :key="index">
					<image :src="item.url" mode=""></image>
				</view>
			</view>
		</view>
		
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				photoData:[{},{},{},{}]
			}
		},
		onLoad(options) {
			const self = this;
			self.user_no = options.user_no;
			self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName ='getProjectToken';
				postData.searchItem = {
					user_type:1,
					user_no:self.user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
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
