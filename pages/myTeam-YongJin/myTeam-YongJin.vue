<template>
	<view>
		<view class="myExtendTop flexColumn center pubBj white pr">
			<view class="money">1520</view>
			<view class="fs13 pdt10">余额(元)</view>
			<view class="txBtn pubColor whiteBj"  @click="Router.navigateTo({route:{path:'/pages/myCashOut/myCashOut'}})">提现</view>
		</view>
		<view class="userBox2 flexRowBetween fs12">
			<view class="child flexColumn">
				<view class="fs15 ftw">120</view>
				<view class="color6">服务佣金</view>
			</view>
			<view class="child flexColumn">
				<view class="fs15 ftw">120</view>
				<view class="color6">推广佣金</view>
			</view>
			<view class="child flexColumn">
				<view class="fs15 ftw">120</view>
				<view class="color6">团队佣金</view>
			</view>
		</view>
		
		<view class="orderNav flexRowBetween borderB1 color6">
			<view class="tt" :class="num==1?'on':''" @click="change('1')">服务佣金</view>
			<view class="tt" :class="num==2?'on':''" @click="change('2')">推广佣金</view>
			<view class="tt" :class="num==3?'on':''" @click="change('3')">团队佣金</view>
		</view>
		
		<view class="">
			<view class="myRowBetween mglr4" v-show="num==1">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="fs13">服务</view>
						<view class="fs12 color6">2020.01.17</view>
					</view>
					<view class="rr red">+56</view>
				</view>
			</view>
			<view class="myRowBetween mglr4" v-show="num==2">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll">
						<view class="fs13">推广</view>
						<view class="fs12 color6">2020.01.18</view>
					</view>
					<view class="rr red">+56</view>
				</view>
			</view>
			<view class="myRowBetween myRowBetweenL70  mglr4" v-show="num==3">
				<view class="item flexRowBetween" v-for="(item,index) in spendData" :key="index">
					<view class="ll flex">
						<view class="photo"><image src="../../static/images/promote-img.png" mode=""></image></view>
						<view class="fs13">员工1</view>
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
			}
			
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/balance.css";
	
</style>
