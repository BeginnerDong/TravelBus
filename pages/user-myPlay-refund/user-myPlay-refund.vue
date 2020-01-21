<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="proRow">
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
				</view>
			</view>
			
			<view class="whiteBj pdlr4 pdt15 pdb15">
				<view class="f5bj">
					<textarea value="" placeholder="填写退款原因" placeholder-class="placeholder" />
				</view>
			</view>
			
		</view>
		
		<view class="submitbtn" style="margin-top:200rpx;">
			<button class="btn" type="button">提交</button>
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
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
		},
	};
</script>

<style>
	@import "../../assets/style/myOrder.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	textarea{color: #222;}
</style>
