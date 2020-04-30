<template>
	<view>
		
		<view class="mglr4 pdt15">
			
			<view class="seltLis fs13 whiteBj pdlr4 radius10">
				<view class="fs15 pdt15 pdb5">推荐支付方式</view>
				<view class="item flexRowBetween" @click="changePay('1')">
					<view class="flex">
						<image class="icon" src="../../static/images/pay-icon.png" mode=""></image>
						<view>支付宝支付</view>
					</view>
					<view><image class="seltIcon" :src="payCurr==1?'../../static/images/pay-icon3.png':'../../static/images/pay-icon2.png'" mode=""></image></view>
				</view>
				<view class="item flexRowBetween" @click="changePay('2')">
					<view class="flex">
						<image class="icon" src="../../static/images/pay-icon1.png" mode=""></image>
						<view>微信支付</view>
					</view>
					<view><image class="seltIcon" :src="payCurr==2?'../../static/images/pay-icon3.png':'../../static/images/pay-icon2.png'" mode=""></image></view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar">
			<view class="mgl15 flex">
				总计<view class="price">{{totalPrice}}</view>
			</view>
			<view class="payBtn" @click="goPay">确认支付</view>
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
				payCurr:1,
				totalPrice:0
			}
		},
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			changePay(payCurr){
				const self = this;
				if(payCurr!=self.payCurr){
					self.payCurr = payCurr
				}
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					thirdapp_id: 2,
					id:self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0]
						self.totalPrice = parseFloat(self.mainData.price)
					}
					self.$Utils.finishFunc('getMainData');
					console.log('self.mainData', self.mainData)
				};
				self.$apis.orderGet(postData, callback);
			},
			
			
			goPay(order_id) {
				const self = this;	
				var nowTime = Date.parse(new Date());
				var postData = {};
				postData = {
					aliPay:{
						price:self.totalPrice
					}
				};
				if(self.payCurr==2){
					postData = {
						wxPay:{
							price:self.totalPrice
						}
					}
				};
				postData.tokenFuncName = 'getUserToken',
				postData.searchItem = {
					id: self.id
				};	
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							if(res.info.package){
								var str=res.info.package
								var arr=str.split("=")[1]
								//胜利ar as1	=arr.toUpperCase()
								//console.log(as1)
								var obj={
									appid:res.info.appId,      //id 应用id
									partnerid:'1578941661',              //商户号 
									prepayid:arr,                         //预支付
									package:'Sign=WXPay',
									noncestr:res.info.nonceStr,
									timestamp:res.info.timeStamp,   //时间戳
									sign:res.info.paySign.substring(0,30)
								}
								var orderInfo = JSON.stringify(obj);
							}else{
								var orderInfo = res.info
							}
							
							console.log(res.info);
							uni.requestPayment({
								
							    provider: self.payCurr==1?'alipay':'wxpay',
							    orderInfo:orderInfo, //微信、支付宝订单数据
							    success: function (res) {
							        console.log('success:' + JSON.stringify(res));
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user-myPlay/user-myPlay'}})
									}, 1000);
							    },
							    fail: function (err) {
							        console.log('fail:' + JSON.stringify(err));
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
							    },
								complete: function (err) {
								    console.log('fail:' + JSON.stringify(err));
								},
							});
							/* const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user-myPlay/user-myPlay'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback); */
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user-myPlay/user-myPlay'}})
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
	@import "../../assets/style/detail.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	
	/* 数量加减 */
	.seltLis .item{padding: 30rpx 0;}
	.seltLis .item .icon{width: 40rpx;height: 40rpx; display: block;margin-right: 20rpx;}
	.seltLis .item .seltIcon{width: 36rpx;height: 36rpx;}
</style>
