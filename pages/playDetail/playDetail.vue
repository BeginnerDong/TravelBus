<template>
	<view>
		
		<view class="detailxqBan">
			<image src="../../static/images/details-img.png" mode=""></image>
		</view>
		<view class="mglr4 pdtb10">
			<view class="">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！仅298元=2大人1小抢牛背梁套票~享含双床房1晚+门票2张+双早+免费停车！</view>
			<view class="mgt5 price fs15 ftw">129</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="mglr4 pdt15">
			<view class="fs15 ftw mgb15">景点详情</view>
			<view class="xqInfor fs13">
				<view class="cont center">
					<view>监管科洛杉矶港讲公开课开发商利君股份各个</view>
					<view>监管科洛杉矶港讲公开课开发商利君股份各个几个卡</view>
					<view>监管科洛杉矶港讲公开课开发商利君股份各个拉金刚认识</view>
					<view><image class="w" src="../../static/images/details-img1.png" mode="widthFix"></image></view>
					<view>估计多少个交流看法的时间</view>
					<view>更进反馈登录手机个分类山沟经历</view>
					<view>国际法大使馆领积分的市公检法是考虑过就过来</view>
				</view>
			</view>
		</view>
		<view class="xqbotomBar pdlr4 pdt10 pdb10">
			<view class="ite">
				<view><image src="../../static/images/details-icon.png" mode=""></image></view>
				<view>客服</view>
			</view>
			<view class="submitbtn" style="width: 72%;">
				<button class="Wbtn" type="button" @click="Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})">立即购买</button>
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
				wx_info:{},
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
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
	
	page{padding-bottom: 160rpx;}
	
	
</style>
