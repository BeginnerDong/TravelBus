<template>
	<view>
		
		<view class="userHead whiteBj pr white">
			
			<view class="center pr" style="line-height: 44px;font-size: 16px;">
				<view @click="prev()" style="position: absolute;top: 50%;left: 0;transform: translateY(-50%);"><image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image></view>
				<view>司机</view>
			</view>
			
			<view class="seltIcon" @click="Router.navigateTo({route:{path:'/pages/user-setUp/user-setUp?type=driver'}})"><image src="../../static/images/about-icon.png" mode=""></image></view>
			<view class="infor flex">
				<image class="photo" :src="userInfoData.mainImg&&userInfoData.mainImg[0]?userInfoData.mainImg[0].url:'../../static/images/about-icon1.png'" mode=""></image>
				<view style="width: 70%;">
					<view class="fs16 pdb5">{{userInfoData.name!=''?userInfoData.name:userInfoData.user_no}}</view>
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
					<switch @change="choose"  :checked="submitData.is_work==1?true:false" style="transform:scale(0.7)" />
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
				wx_info:{},
				userInfoData:{},
				submitData:{
					is_work:0
				},
				time:59
			}
		},
		
		onLoad() {
			const self = this;
			
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
			clearInterval(self.interval)
		},
		
		methods: {
			
			
			choose(e) {
				const self = this;
				if(self.userInfoData.bus_no==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('您暂未绑定车牌', 'none', 1000)
					return
				};
				if(e.target.value){
					self.submitData.is_work = 1
				}else{
					self.submitData.is_work = 0
				}
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
				self.busUpdate()
			},
			
			busUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.userInfoData.bus_no==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('您暂未绑定车牌', 'none', 1000)
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					no: self.userInfoData.bus_no,
					user_type:2
				};
				postData.data = {
					is_work:self.submitData.is_work
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData();
						if(self.submitData.is_work==1){
							self.interval = setInterval(function(){
							  self.getLocation()
							},30000)
						}else{
							clearInterval(self.interval)
						}
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.busUpdate(postData, callback);
			},
			
			getLocation(){
				const self = this;
				uni.getLocation({
				    type: 'wgs84',
				    success: function (res) {
						self.melongitude = res.longitude;
						self.melatitude = res.latitude;
						/* self.melongitude = 108.4994423400
						self.melatitude = 34.3005190100 */
				        console.log('当前位置的经度：' + res.longitude);
				        console.log('当前位置的纬度：' + res.latitude);
						self.busUpdateTwo()
				    }
				});
				self.$Utils.finishFunc('getLocation');
			},
			
			busUpdateTwo() {
				const self = this;
				uni.setStorageSync('canClick', false);
				
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					no: self.userInfoData.bus_no,
					user_type:2
				};
				postData.data = {
					longitude :self.melongitude,
					latitude:self.melatitude
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.busUpdate(postData, callback);
			},
			
			prev(){
				const self = this;
				uni.navigateBack({
					delta:1
				})
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('driverInfo').user_no
				};
				postData.getAfter = {
					bus:{
						tableName:'Bus',
						middleKey:'bus_no',
						key:'no',
						searchItem:{
							status:1
						},
						condition:'=',
						
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];	
						if(self.userInfoData.bus[0]&&self.userInfoData.bus[0].is_work==1){
							self.submitData.is_work=1
							self.interval = setInterval(function(){
							  self.getLocation()
							  //self.time--
							},30000)
						}
					};
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
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.userHead{box-sizing: border-box;background:#fff url(../../static/images/about-img.png) no-repeat 0 0 /100% 100%;padding: 0 4% 60rpx 4%;}
	.userHead .infor{padding-top: 50rpx;}
	.userHead .photo{width: 110rpx;height: 110rpx;border-radius: 50%;margin-right: 20rpx;border: 2px solid #ffecca;;box-sizing: border-box;background: #fff;}
	.seltIcon{position: absolute;top: 100rpx;right: 4%;}
	.seltIcon image{width: 36rpx;height: 36rpx;display: block;}
	.myRowBetween .item{background: #fff;padding: 30rpx 4%;}
	.myRowBetween .ll .icon{width: 32rpx;height: 32rpx;margin-right: 20rpx;}
</style>
