<template>
	<view>
		
		<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15">
			<view class="tt" :class="current==1?'on':''" @click="changeCurr('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="changeCurr('2')">待付款</view>
			<view class="tt" :class="current==3?'on':''" @click="changeCurr('3')">待完成</view>
			<view class="tt" :class="current==4?'on':''" @click="changeCurr('4')">已完成</view>
			<view class="tt" :class="current==5?'on':''" @click="changeCurr('5')">退款</view>
		</view>
		
		<view class="mglr4">
			<view class="proRow">
				<view class="item" v-for="item in mainData">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：{{item.create_time}}</view>
						<view class="red">待付款</view>
						<view class="red" v-if="item.pat_status==0">待付款</view>
						<view class="red" v-if="item.pat_status==1&&item.transport_status==0">待完成</view>
						<view class="red" v-if="item.pat_status==1&&item.transport_status==2">已完成</view>
						<view class="red" v-if="item.order_step==1||item.order_step==2">退款</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.orderItem&&item.orderItem[0]&&item.orderItem[0].snap_product&&
							item.orderItem[0].snap_product.mainImg&&item.orderItem[0].snap_product.mainImg[0]?item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow4 fs12">{{item.title}}</view>
						</view>
					</view>
					<view class="dashedB1 mgt15"></view>
					<view class="flexRowBetween pdt15">
						<view class="flex">
							<view class="price mgr10">{{item.price}}</view>
							<view class="fs12 color6">×{{item.count}}</view>
						</view>
						<view class="underBtn flexEnd" v-if="item.pat_status==0">
							<view class="Bbtn">去支付</view>
						</view>
						
						<view class="underBtn flexEnd" v-if="item.order_step==1||item.order_step==2">
							<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user-myPlay-refund/user-myPlay-refund'}})">退款</view>
						</view>
					</view>
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
				searchItem:{
					type:1,
					thirdapp_id:2
				},
				willId:-1,
				current:1,
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
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
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			orderUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					transport_status:2
				};
				postData.searchItem = {
					id:self.willId,
				};
				const callback = (data) => {
					uni.setStorageSync('canClick', true);
					if (data && data.solely_code == 100000) {
						self.$Utils.showToast('操作成功','none');
						setTimeout(function() {
							self.getMainData(true)
						}, 1000);
					} else {
						self.$Utils.showToast(data.msg,'none')
					}
				};
				self.$apis.orderUpdate(postData, callback);
			 },
			
			changeCurr(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						delete self.searchItem.transport_status;
						delete self.searchItem.pay_status;
						delete self.searchItem.order_type;
					}else if(self.current==2){
						self.searchItem.pay_status = 0
						delete self.searchItem.order_type;
					}else if(self.current==3){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 0
						delete self.searchItem.order_type;
					}else if(self.current==4){
						self.searchItem.pay_status = 1
						self.searchItem.transport_status = 2;
						delete self.searchItem.order_type;
					}else if(self.current==5){
						delete self.searchItem.transport_status;
						delete self.searchItem.pay_status;
						self.searchItem.order_step = ['in',[1,2]]
					}
					self.getMainData(true)
				}
			},
		},
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
</style>
