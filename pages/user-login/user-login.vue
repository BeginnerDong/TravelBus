<template>
	<view>
	
		<view class="loginCont">
			<view class="fs18 ftw title">账号登录</view>
			
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon.png" mode=""></image></view>
				<view class="input"><input type="text" v-model="submitData.login_name" placeholder="手机号" placeholder-class="placeholder"/></view>
			</view>
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon1.png" mode=""></image></view>
				<view class="input"><input type="password" v-model="submitData.password" placeholder="密码" placeholder-class="placeholder"/></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="submit">登录</button>
		</view>
		<view style="margin: 0 auto;width: 80%;" @click="Router.navigateTo({route:{path:'/pages/user-register/user-register'}})">没有账号，去注册</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				submitData:{
					login_name:'',
					password:''
				},
				showAll:false
			}
		},
		
		onLoad(options) {
			const self = this;
			
		},
		
		methods: {
			
			submit() {
				const self = this;
			
				const postData = {
					login_name: self.submitData.login_name,
					password:self.submitData.password
				};
				if (self.$Utils.checkComplete(self.submitData)) {
					
					const callback = (res) => {
						if (res.solely_code == 100000) {
							console.log(res);
							uni.setStorageSync('userToken', res.token);
							uni.setStorageSync('userInfo', res.info);
							uni.navigateBack({
								delta:1
							})
						} else {
							self.$Utils.showToast(res.msg,'none')
						}
					}
					self.$apis.userLogin(postData, callback);
				} else {
					self.$Utils.showToast('请补全登录信息', 'none')
				};
			},
			
		},
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
