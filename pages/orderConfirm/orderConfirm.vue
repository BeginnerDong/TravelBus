<template>
	<view>
		
		<view class="mglr4 pdt15"  v-for="item in mainData">
			
			<view class="pdlr4 pdt15 pdb15 whiteBj">
				<view class="color6 fs13 mgb10">订单描述</view>
				<view class="">{{item.product&&item.product.title?item.product.title:''}}</view>
			</view>
			
			<view class="radius10 whiteBj mgt15 editLine">
				<view class="item flexRowBetween">
					<view style="width: 25%;">时间选择：</view>
					<picker mode="date" @change="changeTime" style="width: 70%;">{{submitData.time!=''?submitData.time:'请选择'}}</picker>
					<view><image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
				</view>
				<view class="item flex">
					<view class="ll">购买数量：</view>
					<view class="rr flex">
						<view class="numBox flex">
							<view @click="counter('-')">-</view>
							<view class="num pubColor">{{item.count}}</view>
							<view @click="counter('+')">+</view>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">留言：</view>
					<view class="rr flex">
						<input type="text" v-model="submitData.passage1" placeholder="请输入留言" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="radius10 whiteBj mgt15 editLine">
				<view class="mglr4 pdt15 pdb5 fs13 color6">订单联系人</view>
				
				<view class="item flex">
					<view class="ll">姓名：</view>
					<view class="rr flex">
						<input type="text" v-model="submitData.name" placeholder="请输入姓名" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">手机：</view>
					<view class="rr flex">
						<input type="number" maxlength="11" v-model="submitData.phone" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar">
			<view class="mgl15 flex">
				总计<view class="price">{{totalPrice}}</view>
			</view>
			<view class="payBtn" @click="$Utils.stopMultiClick(submit)" >提交订单</view>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info: {},
				is_show: false,
				is_tmptShow: false,
				count: 1,
				wechat:'',
				mainData:[],
				addressData:{},
				totalPrice:0,
				pay:{},
				submitData:{
					name:'',
					phone:'',
					
					passage1:'',
					time:''
				}
			}
		},
		onLoad() {
			const self = this;
			self.mainData = uni.getStorageSync('payPro');
			self.countTotalPrice()
		},

		onShow() {
			const self = this;
			/* if (uni.getStorageSync('choosedAddressData')) {
				self.addressData = uni.getStorageSync('choosedAddressData')

			} else {
				self.getAddressData()
			}; */
		},

		methods: {
			
			changeTime(e){
				const self = this;
				self.submitData.time = e.detail.value;
			},


			counter(type) {
				const self = this;
				if (type == '+') {
					self.mainData[0].count++;
				} else {
					if (self.mainData[0].count > 1) {
						self.mainData[0].count--;
					}
				};
				self.countTotalPrice();
			},

			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price * self.mainData[i].count
				};
				var wxPay = parseFloat(self.totalPrice).toFixed(2)
				//console.log('wxPay',wxPay)
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay,
					};
				} else {
					delete self.pay.wxPay;
				};
				console.log(self.pay)
			},

			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var data = self.$Utils.cloneForm(self.submitData)
				delete data.passage1;
				var pass = self.$Utils.checkComplete(data);
				if (!pass) {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全必要信息', 'none')
					return
				};
				
				
				var orderList = [{
					product_id: self.mainData[0].product_id,
					count: self.mainData[0].count,
					type: 1,
					data: data,
					//snap_address: self.addressData
				}];
				self.addOrder(orderList)
			},
			
			addOrder(orderList) {
				const self = this;	
				/* if(self.orderId){
					self.goPay()
					return
				}; */
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(orderList);
				postData.data = {};
				postData.tokenFuncName = 'getUserToken';
				const callback = (res) => {
					uni.setStorageSync('canClick', true);
					if (res && res.solely_code == 100000) {
						//self.orderId = res.info.id;
						//self.goPay()
						self.Router.navigateTo({route:{path:'/pages/orderPay/orderPay?id='+res.info.id}})
					} else {		
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			goPay(order_id) {
				const self = this;	
				uni.setStorageSync('canClick',false);
				
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getUserToken',
				postData.searchItem = {
					id: self.orderId
				};	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										
										self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								
								self.$Router.redirectTo({route:{path:'/pages/userOrder/userOrder'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.editLine .item{padding: 30rpx 4%;}
	
	/* 数量加减 */
	.numBox{border:1px solid #eee; border-radius: 8rpx;}
	.numBox view{ width: 60rpx; height: 44rpx;color: #999; font-size:32rpx ; text-align: center; line-height: 44rpx;}
	.numBox view.num{ width: 80rpx; color: #333; font-size: 26rpx;border-left:1px solid #eee;border-right:1px solid #eee; color: #0591fa;}
</style>
