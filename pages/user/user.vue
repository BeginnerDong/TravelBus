<template>
	<view>

		<view class="userHead whiteBj pr white">
			<view class="center" style="line-height: 44px;font-size: 16px;">我的</view>
			<view class="seltIcon" @click="Router.navigateTo({route:{path:'/pages/user-setUp/user-setUp'}})">
				<image src="../../static/images/about-icon.png" mode=""></image>
			</view>
			<view class="infor flex" v-if="isLogin">
				<image class="photo" :src="userInfoData.mainImg&&userInfoData.mainImg[0]?userInfoData.mainImg[0].url:'../../static/images/about-icon1.png'" mode=""></image>
				<view style="width: 70%;">
					<view class="fs16 pdb5">{{userInfoData.name!=''?userInfoData.name:userInfoData.phone}}</view>
				</view>
				
			</view>
			<view class="infor flex" v-else>
				<view>
					<view class="fs16 pdb5" @click="Router.navigateTo({route:{path:'/pages/user-login/user-login'}})">请点击登录</view>
				</view>
			</view>
		</view>
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-if="isLogin" @click="Router.navigateTo({route:{path:'/pages/user-myPlay/user-myPlay'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">周边游</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" v-if="!isLogin" @click="showToast">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon2.png" mode=""></image>
					<view class="">周边游</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" v-if="isLogin" @click="Router.navigateTo({route:{path:'/pages/user-byCar/user-byCar'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view class="">包车</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			
			<view class="item flexRowBetween" v-if="!isLogin" @click="showToast">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon3.png" mode=""></image>
					<view class="">包车</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/user-contactUs/user-contactUs'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon4.png" mode=""></image>
					<view class="">联系我们</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item mgt10 flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/driver-login/driver-login'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/about-icon5.png" mode=""></image>
					<view class="">司机端登录</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
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
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/play/play'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">周边游</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">智能包车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})">
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
				Router: this.$Router,
				isLogin:false,
				userInfoData:{}
			}
		},
		
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},

		onShow() {
			const self = this;
			if (uni.getStorageSync('userInfo')) {
				self.isLogin = true
				self.$Utils.loadAll(['getUserInfoData'], self);
			}
		},

		methods: {
			
			showToast(){
				const self = this;
				self.$Utils.showToast('您暂未登录','none')
			},

			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('userInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];	
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";

	page {
		padding-bottom: 140rpx;
		background: #F5F5F5;
	}

	.userHead {
		box-sizing: border-box;
		background: #fff url(../../static/images/about-img.png) no-repeat 0 0 /100% 100%;
		padding: 0 4% 60rpx 4%;
	}

	.userHead .infor {
		padding-top: 50rpx;
	}

	.userHead .photo {
		width: 110rpx;
		height: 110rpx;
		border-radius: 50%;
		margin-right: 20rpx;
		border: 2px solid #ffecca;
		;
		box-sizing: border-box;
		background: #fff;
	}

	.seltIcon {
		position: absolute;
		top: 100rpx;
		right: 4%;
	}

	.seltIcon image {
		width: 36rpx;
		height: 36rpx;
		display: block;
	}

	.myRowBetween .item {
		background: #fff;
		padding: 30rpx 4%;
	}

	.myRowBetween .ll .icon {
		width: 32rpx;
		height: 32rpx;
		margin-right: 20rpx;
	}
</style>
