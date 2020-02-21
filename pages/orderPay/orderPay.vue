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
				总计<view class="price">66.8</view>
			</view>
			<view class="payBtn">确认支付</view>
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
				payCurr:1
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
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
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
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
