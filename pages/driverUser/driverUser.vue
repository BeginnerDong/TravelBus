<template>
	<view>

		<view class="userHead whiteBj pr white">

			<view class="center pr" style="line-height: 44px;font-size: 16px;">
				<view @click="prev()" style="position: absolute;top: 50%;left: 0;transform: translateY(-50%);">
					<image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL.png" mode=""></image>
				</view>
				<view>司机</view>
			</view>

			<view class="seltIcon" @click="Router.navigateTo({route:{path:'/pages/user-setUp/user-setUp?type=driver'}})">
				<image src="../../static/images/about-icon.png" mode=""></image>
			</view>
			<view class="infor flex">
				<image class="photo" :src="userInfoData.mainImg&&userInfoData.mainImg[0]?userInfoData.mainImg[0].url:'../../static/images/about-icon1.png'"
				 mode=""></image>
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
					<switch @change="choose" :checked="submitData.is_work==1?true:false" style="transform:scale(0.7)" />
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/driverUser-bindingPlates/driverUser-bindingPlates'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/driver-icon1.png" mode=""></image>
					<view class="">绑定车牌</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/driverUser-salfSetting/driverUser-salfSetting'}})">
				<view class="ll flex">
					<image class="icon" src="../../static/images/driver-icon2.png" mode=""></image>
					<view class="">安全设置</view>
				</view>
				<view class="rr">
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
		</view>
		<view>当前经度:{{melongitude}},当前纬度:{{melatitude}}</view>
		<view>getLocation次数:{{getLocationTime}}</view>
		<view>update次数:{{time}}</view>
		<view>错误信息：{{errMsg}}+{{errTime}}</view>
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
	const leleLocation = uni.requireNativePlugin('Lele-Location');
	export default {
		data() {
			return {
				Router: this.$Router,
				showView: false,
				score: '',
				wx_info: {},
				userInfoData: {},
				submitData: {
					is_work: 0
				},
				melongitude: '',
				melatitude: '',
				time: 0,
				errMsg:'暂无',
				errTime:0,
				getLocationTime:0,
				isAjax:true
			}
		},

		onLoad() {
			const self = this;
			
		},

		onShow() {
			const self = this;
			if (uni.getStorageSync('intervalId')) {
				clearInterval(uni.getStorageSync('intervalId'));
			};
			uni.removeStorageSync('intervalId');
			self.$Utils.loadAll(['getUserInfoData'], self);

		},



		methods: {

			 
			//允许程序后台运行，以持续获取GPS位置  
			wakeLock() {  
			    //Android  
				var g_wakelock = null; 
			    var main = plus.android.runtimeMainActivity();  
			    var Context = plus.android.importClass("android.content.Context");  
			    var PowerManager = plus.android.importClass("android.os.PowerManager");  
			    var pm = main.getSystemService(Context.POWER_SERVICE);  
			    g_wakelock = pm.newWakeLock(PowerManager.PARTIAL_WAKE_LOCK, "ANY_NAME");  
			    g_wakelock.acquire();  
			},
			
			getLocationPermission() {
				let self = this
				console.log('start')
				uni.getLocation({
					type: 'wgs84',
					success: function(res) {
						uni.getSystemInfo({
							success: function(res) {
								if (res.platform === "android") {
									
									self.location()
								}
							}
						});
					}
				});
			},
			location() {
				//开始定位
				const self = this;
				leleLocation.location({
					 sendUpUrl: "http://106.12.155.217/bus/public/index.php/api/v1/Project/Solely/updateCar", //上送服务器的接口地址，请求方式为post，isSendUp为ture时必传
					token: "", //可选，令牌，根据服务端要求
					userId: self.userInfoData.bus_no, //可选，用户标识，默认为null
					userIdKey: "bus_no", //上送服务端的用户标识key值，对应值是userId传入的值，不指定userIdKey时，默认key为"userId"字符串。
					intervalTime: 10000, //可选，间隔定位时间，单位毫秒(ms)，默认2000ms。
					isSendUp: true, //是否需要上送到业务服务器
					customParams: {
						
					} //自定义上送服务器端的参数，isSendUp为true时才有意义。
				}, result => {
					console.log(result)
					const msg = result;
					
					//self.$Utils.showToast(msg)
					switch (result.code) {
						case '0':
							self.melongitude = JSON.parse(result.msg).longitude;
							self.melatitude = JSON.parse(result.msg).latitude;
							self.time++
							//console.log('callback---msg--' + result.msg);
							//console.log('callback---sendContent--' + result.sendContent);
							break;
						case '-1':
							//console.log("callback---button--" + result.msg);
							break;
					}
				});
			},

			choose(e) {
				const self = this;
				if (self.userInfoData.bus_no == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('您暂未绑定车牌', 'none', 1000)
					return
				};
				if (e.target.value) {
					self.submitData.is_work = 1
				} else {
					self.submitData.is_work = 0
				}
				console.log('switch2 发生 change 事件，携带值为', e.target.value)
				self.busUpdate()
			},

			busUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if (self.userInfoData.bus_no == '') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('您暂未绑定车牌', 'none', 1000)
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					no: self.userInfoData.bus_no,
					user_type: 2
				};
				postData.data = {
					is_work: self.submitData.is_work
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData();
						uni.setStorageSync('isWork', self.submitData.is_work);
						if (self.submitData.is_work == 1) {
							//self.getLocation()
							/* self.interval = setInterval(function(){
							  self.getLocation()
							  
							},3000)
							uni.setStorageSync('intervalId', self.interval) */
							//self.getLocationPermission()
						} else {
							clearInterval(uni.getStorageSync('intervalId'));
							uni.removeStorageSync('intervalId');
							//leleLocation.stop()
						}

					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
				};
				self.$apis.busUpdate(postData, callback);
			},

			getLocation() {
				const self = this;
				/* self.melongitude = '';
				self.melatitude = ''; */
				uni.getLocation({
					type: 'wgs84',
					success: function(res) {
						console.log(res)
						self.getLocationTime++
						console.log('self.melongitude',res.longitude)
						console.log('self.melatitude',res.latitude)
						if(uni.getStorageSync('carLatitude')!=res.latitude||uni.getStorageSync('carLongitude')!=res.longitude){
							self.melongitude = res.longitude;
							self.melatitude = res.latitude;
							uni.setStorageSync('carLatitude',res.latitude);
							uni.setStorageSync('carLongitude',res.longitude);
							self.busUpdateTwo()
						};
					},
					fail(res) {
						console.log(res)
					}
				});
				self.$Utils.finishFunc('getLocation');
			},

			busUpdateTwo() {
				const self = this;
				if(!self.isAjax){
					return
				};
				self.isAjax = false;
				uni.setStorageSync('canClick', false);
				
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					no: self.userInfoData.bus_no,
					user_type: 2
				};
				postData.data = {
					longitude: self.melongitude,
					latitude: self.melatitude
				};
				
				const callback = (data) => {
					console.log('data',data);
					self.isAjax = true;
					if (data.solely_code == 100000) {
						self.time++
						console.log(self.time)
						uni.setStorageSync('canClick', true);
					} else {
						//clearInterval(uni.getStorageSync('intervalId'));
						uni.setStorageSync('canClick', true);
						
						
						self.$Utils.showToast(data.msg, 'none', 1000);
						self.errMsg = data.msg;
						self.errTime ++
					}
				};
				self.$apis.busUpdate(postData, callback);
			},

			prev() {
				const self = this;
				uni.navigateBack({
					delta: 1
				})
			},

			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('driverInfo').user_no
				};
				postData.getAfter = {
					bus: {
						tableName: 'Bus',
						middleKey: 'bus_no',
						key: 'no',
						searchItem: {
							status: 1
						},
						condition: '=',

					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						if (self.userInfoData.bus[0] && self.userInfoData.bus[0].is_work == 1) {
							self.submitData.is_work = 1
							uni.setStorageSync('isWork', self.submitData.is_work);
							self.getLocation();
							self.interval = setInterval(function() {
								self.getLocation()
								//self.time++
								 //console.log(self.time)
							}, 3000);
							uni.setStorageSync('intervalId', self.interval)
							//self.getLocationPermission()
							//self.getLocation()
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
