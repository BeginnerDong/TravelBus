<template>
	<view>
		
		<view class="myRowBetween pdlr4 whiteBj">
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-uploadPhoto/user-uploadPhoto'}})" >
				<view class="ll">图像</view>
				<view class="rr flexEnd">
					<view class="userPhoto"><image src="../../static/images/about-icon1.png" mode=""></image></view>
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-nickname/user-setUp-nickname'}})" >
				<view class="ll">昵称</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">请输入昵称</view>
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-setUp-phone/user-setUp-phone'}})" >
				<view class="ll">联系方式</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">请输入联系方式</view>
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.userPhoto{width: 80rpx;height: 80rpx;border-radius: 50%;border: 1px solid #eee;box-sizing: border-box;overflow: hidden;}
	.userPhoto image{width: 100%;height: 100%; display: block;}
	.myRowBetween .item:last-child{border-bottom: 0;}
</style>
