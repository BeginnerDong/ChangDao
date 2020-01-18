<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white pr">
			<view class="money">1520</view>
			<view class="fs13 pdt10">余额(元)</view>
			<view class="txBtn pubColor whiteBj"  @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
			<view class="ewmBtn flexEnd fs12" @click="ewmShow">
				<image class="icon" src="../../static/images/promote-icon.png" mode=""></image>
				推广二维码
			</view>
		</view>
		<view class="orderNav flexRowBetween borderB1 color6">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">佣金明细</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">推广客户</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4" v-show="num==1">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="fs13">推广</view>
						<view class="fs12 color6">2020.01.17</view>
					</view>
					<view class="rr red">+56</view>
				</view>
			</view>
			<view class="myRowBetween myRowBetweenL70  mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll flex">
						<view class="photo"><image src="../../static/images/promote-img.png" mode=""></image></view>
						<view class="fs13">签到</view>
					</view>
					<view class="rr color9">2020.01.17</view>
				</view>
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
				num:1,
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
			change(num){
				const self = this;
				if(num!= self.num){
					self.num = num
				}
			},
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
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/balance.css";
	
</style>
