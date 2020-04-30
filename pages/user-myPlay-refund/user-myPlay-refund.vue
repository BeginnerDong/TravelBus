<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：{{mainData.create_time}}</view>
						<view class="red">已付款</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow4 fs12">{{mainData.title}}</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="whiteBj pdlr4 pdt15 pdb15">
				<view class="f5bj">
					<textarea v-model="passage2" placeholder="填写退款原因" placeholder-class="placeholder" />
				</view>
			</view>
			
		</view>
		
		<view class="submitbtn" style="margin-top:200rpx;">
			<button class="btn" type="button" @click="refund">提交</button>
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
				passage2:''
			}
		},
		
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					thirdapp_id: 2,
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
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
					}
					self.$Utils.finishFunc('getMainData');
					console.log('self.mainData', self.mainData)
				};
				self.$apis.orderGet(postData, callback);
			},
			
			refund() {
				const self = this;		
				const postData = {};			
				postData.searchItem = {
					order_no:self.mainData.order_no
				};		
				postData.data = {
					passage1:self.passage2,
					order_step:1
				};
				postData.tokenFuncName = 'getUserToken';			
				const callback = (res) => {
					if(res.solely_code==100000){
						self.$Utils.showToast('申请成功','none',1000);	
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
					}else{
						self.$Utils.showToast(res.msg,'none');
					}
				};
				self.$apis.orderUpdate(postData, callback);
			},
			
		},
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	textarea{color: #222;}
</style>
