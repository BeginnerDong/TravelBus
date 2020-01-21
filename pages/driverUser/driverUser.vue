<template>
	<view>
		
		<view class="userHead whiteBj pr white">
			
			<view class="center pr" style="line-height: 44px;font-size: 16px;">
				<view @click="prev()" style="position: absolute;top: 50%;left: 0;transform: translateY(-50%);"><image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image></view>
				<view>司机</view>
			</view>
			
			<view class="seltIcon" @click="Router.navigateTo({route:{path:'/pages/user-setUp/user-setUp'}})"><image src="../../static/images/about-icon.png" mode=""></image></view>
			<view class="infor flex">
				<image class="photo" src="../../static/images/about-icon1.png" mode=""></image>
				<view style="width: 70%;">
					<view class="fs16 pdb5">夏日露水</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween">
			<view class="item flexRowBetween" style="padding-right: 10rpx;">
				<view class="ll flex">
					<image class="icon" src="../../static/images/driver-icon.png" mode=""></image>
					<view class="">定位</view>
				</view>
				<view class="rr flexEnd">
					<switch checked="true" style="transform:scale(0.7)" />
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/driverUser-bindingPlates/driverUser-bindingPlates'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/driver-icon1.png" mode=""></image>
					<view class="">绑定车牌</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/driverUser-salfSetting/driverUser-salfSetting'}})" >
				<view class="ll flex">
					<image class="icon" src="../../static/images/driver-icon2.png" mode=""></image>
					<view class="">安全设置</view>
				</view>
				<view class="rr"><image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
			</view>
		</view>
			
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/play/play'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">周边游</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">智能包车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4-a.png" />
				</view>
				<view class="text this-text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
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
			prev(){
				const self = this;
				self.$router.go(-1)
			}
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.userHead{box-sizing: border-box;background:#fff url(../../static/images/about-img.png) no-repeat 0 0 /100% 100%;padding: 0 4% 60rpx 4%;}
	.userHead .infor{padding-top: 50rpx;}
	.userHead .photo{width: 110rpx;height: 110rpx;border-radius: 50%;margin-right: 20rpx;border: 2px solid #ffecca;;box-sizing: border-box;background: #fff;}
	.seltIcon{position: absolute;top: 100rpx;right: 4%;}
	.seltIcon image{width: 36rpx;height: 36rpx;display: block;}
	.myRowBetween .item{background: #fff;padding: 30rpx 4%;}
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
</style>
