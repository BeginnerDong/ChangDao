<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white">
			<view class="money">1520</view>
			<view class="fs13 pdt10">余额(元)</view>
		</view>
		<view class="userBox2 boxShaow flexRowBetween radius10 whiteBj">
			<view class="">产生利息</view>
			<view class="red">15.6元</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="fs26">充值</view>
						<view class="fs12 color6">2020.01.17</view>
					</view>
					<view class="rr red">+56</view>
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
				showView: false,
				score:'',
				wx_info:{},
				num:1,
				spendData:[{},{},{}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
				}
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/balance.css";
	
</style>
