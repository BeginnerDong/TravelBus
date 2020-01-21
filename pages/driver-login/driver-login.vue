<template>
	<view>
	
		<view class="loginCont">
			<view class="fs18 ftw title">账号登录</view>
			
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon.png" mode=""></image></view>
				<view class="input"><input type="text" value="" placeholder="手机号" placeholder-class="placeholder"/></view>
			</view>
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon1.png" mode=""></image></view>
				<view class="input"><input type="password" value="" placeholder="密码" placeholder-class="placeholder"/></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="Router.navigateTo({route:{path:'/pages/driverUser/driverUser'}})">登录</button>
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
			
		}
	};
</script>

<style>
	.loginCont{padding: 70rpx 10%;}
	.loginCont .title{padding-bottom: 20rpx;}
	
	.item{border-bottom: 1px solid #eee;padding: 60rpx 0 20rpx 0;}
	.item .icon{width: 34rpx;height: 34rpx;margin-right: 20rpx;}
	.item .icon image{width: 100%;height: 34rpx;display: block;}
	.item .input{width: 88%;}
	.item .input input{width: 100%;height: 40rpx;display: block;font-size: 26rpx;}
</style>
