<template>
	<view>

		<view class="myRowBetween pdlr4 whiteBj">
			<view class="item flexRowBetween">
				<view class="ll">图像</view>
				<view class="rr flexEnd">
					<view class="userPhoto" @click="upLoadImg('mainImg')" v-if="submitData.mainImg.length>0">
						<image :src="submitData.mainImg[0].url" mode=""></image>
					</view>
					<view class="userPhoto" @click="upLoadImg('mainImg')" v-else>
						<image  src="../../static/images/about-icon1.png" mode=""></image>
					</view>
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" 
			@click="Router.navigateTo({route:{path:'/pages/user-setUp-nickname/user-setUp-nickname?name='+userInfoData.name+'&type=driver'}})">
				<view class="ll">昵称</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">{{userInfoData.name!=''?userInfoData.name:'请输入昵称'}}</view>
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
			<view class="item flexRowBetween" 
			@click="Router.navigateTo({route:{path:'/pages/user-setUp-phone/user-setUp-phone?name='+userInfoData.phone+'&type=driver'}})">
				<view class="ll">联系方式</view>
				<view class="rr flexEnd">
					<view class="color9 fs13">{{userInfoData.phone!=''?userInfoData.phone:'请输入联系方式'}}</view>
					<image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="submint"  @click="loginOff">退出登录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				submitData:{
					mainImg:[]
				},
				userInfoData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.type){
				self.type = options.type
			};
			//self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		onShow() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			loginOff(){
				const self = this;
				if(self.type&&self.type=='driver'){
					uni.removeStorageSync('driverToken');
					uni.removeStorageSync('driverInfo');
					self.Router.reLaunch({route:{path:'/pages/user/user'}})
				}else{
					uni.removeStorageSync('userToken');
					uni.removeStorageSync('userInfo');
					self.Router.redirectTo({route:{path:'/pages/user/user'}})
				}
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('userInfo').user_no
				};
				if(self.type&&self.type=='driver'){
					postData.tokenFuncName = 'getDriverToken';
					postData.searchItem = {
						user_no: uni.getStorageSync('driverInfo').user_no
					};
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
						self.submitData.mainImg = self.userInfoData.mainImg;
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			upLoadImg(type) {
				const self = this;			
				uni.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData[type] = [];
						self.submitData[type].push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						self.userInfoUpdate()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				uni.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: self.type?'getDriverToken':'getUserToken',
							
						}, callback)
					},
					fail: function(err) {
						uni.hideLoading();
					},			
				})			
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('userInfo').user_no
				};
				if(self.type&&self.type=='driver'){
					postData.tokenFuncName = 'getDriverToken';
					postData.searchItem = {
						user_no: uni.getStorageSync('driverInfo').user_no
					};
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData()
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/navbar.css";

	page {
		padding-bottom: 60rpx;
		background: #F5F5F5;
	}

	.userPhoto {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		border: 1px solid #eee;
		box-sizing: border-box;
		overflow: hidden;
	}

	.userPhoto image {
		width: 100%;
		height: 100%;
		display: block;
	}

	.myRowBetween .item:last-child {
		border-bottom: 0;
	}
</style>
