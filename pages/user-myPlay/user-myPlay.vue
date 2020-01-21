<template>
	<view>
		
		<view class="orderNav whiteBj flexRowBetween boxShaow color6 mgb15">
			<view class="tt" :class="curr==1?'on':''" @click="changeCurr('1')">全部</view>
			<view class="tt" :class="curr==2?'on':''" @click="changeCurr('2')">待付款</view>
			<view class="tt" :class="curr==3?'on':''" @click="changeCurr('3')">已完成</view>
			<view class="tt" :class="curr==4?'on':''" @click="changeCurr('4')">已完成</view>
			<view class="tt" :class="curr==5?'on':''" @click="changeCurr('5')">退款</view>
		</view>
		
		<view class="mglr4">
			<view class="proRow">
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">待付款</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/orderPaly-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow4 fs12">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！仅298元=2大人1小抢牛背梁套票~享含双床房1晚+门票2张+双早+免费停车！</view>
						</view>
					</view>
					<view class="dashedB1 mgt15"></view>
					<view class="flexRowBetween pdt15">
						<view class="flex">
							<view class="price mgr10">56.00</view>
							<view class="fs12 color6">×1</view>
						</view>
						<view class="underBtn flexEnd">
							<view class="Bbtn">去支付</view>
						</view>
					</view>
				</view>
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">待完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/orderPaly-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow4 fs12">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！仅298元=2大人1小抢牛背梁套票~享含双床房1晚+门票2张+双早+免费停车！</view>
						</view>
					</view>
					<view class="dashedB1 mgt15"></view>
					<view class="flexRowBetween pdt15">
						<view class="flex">
							<view class="price mgr10">56.00</view>
							<view class="fs12 color6">×1</view>
						</view>
						<view class="underBtn flexEnd">
							<view class="Bbtn" @click="Router.navigateTo({route:{path:'/pages/user-myPlay-refund/user-myPlay-refund'}})">退款</view>
						</view>
					</view>
				</view>
				
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">已完成</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/orderPaly-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow4 fs12">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！仅298元=2大人1小抢牛背梁套票~享含双床房1晚+门票2张+双早+免费停车！</view>
						</view>
					</view>
					<view class="dashedB1 mgt15"></view>
					<view class="flexRowBetween pdt15">
						<view class="flex">
							<view class="price mgr10">56.00</view>
							<view class="fs12 color6">×1</view>
						</view>
					</view>
				</view>
				
				<view class="item">
					<view class="fs12 flexRowBetween mgb10">
						<view class="color9">交易时间：2020-01-18</view>
						<view class="red">退款</view>
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image src="../../static/images/orderPaly-img1.png" mode=""></image>
						</view>
						<view class="infor">
							<view class="avoidOverflow4 fs12">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！仅298元=2大人1小抢牛背梁套票~享含双床房1晚+门票2张+双早+免费停车！</view>
						</view>
					</view>
					<view class="dashedB1 mgt15"></view>
					<view class="flexRowBetween pdt15">
						<view class="flex">
							<view class="price mgr10">56.00</view>
							<view class="fs12 color6">×1</view>
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
				showView: false,
				score:'',
				wx_info:{},
				curr:1
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!= self.curr){
					self.curr = curr
				}
			}
		},
	};
</script>

<style>
	@import "../../assets/style/orderNav.css";
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
</style>
