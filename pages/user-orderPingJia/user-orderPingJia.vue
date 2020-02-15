<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：{{mainData.create_time}}</view>
						<view class="red">已使用/已完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="mainData.orderItem&&mainData.orderItem[0]&&mainData.orderItem[0].snap_product&&
						mainData.orderItem[0].snap_product.mainImg&&mainData.orderItem[0].snap_product.mainImg[0]?mainData.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow2 fs13">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.title:''}}</view>
							<view class="B-price flexRowBetween">
								<view class="price">{{mainData.orderItem&&mainData.orderItem[0]&&
						mainData.orderItem[0].snap_product?mainData.orderItem[0].snap_product.price:''}}</view>
								<view class="fs13">×{{mainData.count}}</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="whiteBj pdlr4 pdt15 pdb15">
				<view>评价</view>
				<view class="mgt10 f5bj">
					<textarea v-model="submitData.description"  placeholder="请输入" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		<view class="submitbtn" style="margin-top:200rpx;">
			<button class="btn" type="button"  @click="Utils.stopMultiClick(submit)">确定</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				mainData:{},
				submitData: {
					order_no: '',
					relation_id: '',
					
					description: '',
					type:1,
					title:'',
					mainImg:[]
				},
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			var res = self.$Token.getProjectToken(function(){
				self.$Utils.loadAll(['getMainData'], self)
			});
			if(res){
				self.$Utils.loadAll(['getMainData'], self)
			};
			self.submitData.title = uni.getStorageSync('user_info').nickname
			self.submitData.mainImg = [{type:'image',url:uni.getStorageSync('user_info').headImgUrl}]
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					id:self.id
				};
				postData.getAfter = {
					orderItem:{
						tableName:'OrderItem',
						middleKey:'order_no',
						key:'order_no',
						searchItem:{
							status:1
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.mainData = res.info.data[0];
						self.submitData.order_no = self.mainData.order_no;
						self.submitData.relation_id = self.mainData.product_id
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getMainData');
					
				};
				self.$apis.orderGet(postData, callback);
			},
			
			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					self.messageAdd();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},
			
			messageAdd() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.saveAfter = [{
				  tableName:'Order',
				  FuncName:'update',
				  searchItem:{
				    id:self.id
				  },
				  data:{
				    isremark:1,
				  }
				}];
				const callback = (data) => {
			
					if (data.solely_code == 100000) {
			
						self.$Utils.showToast('评价成功', 'none')
						setTimeout(function() {
							uni.navigateBack({
								delta: 1
							})
						}, 1000);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
			
				};
				self.$apis.messageAdd(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{background: #F5F5F5;}
	textarea{color: #222;}
</style>
