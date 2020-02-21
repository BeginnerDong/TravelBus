<template>
	<view>
	
		<view class="loginCont">
			<view class="fs18 ftw title">账号注册</view>
			
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon.png" mode=""></image></view>
				<view class="input"><input type="text" v-model="submitData.phone" placeholder="手机号" placeholder-class="placeholder"/></view>
			</view>
			<view class="item flex">
				<view class="icon"><image src="../../static/images/login-icon1.png" mode=""></image></view>
				<view class="input"><input type="password" v-model="submitData.password" placeholder="密码" placeholder-class="placeholder"/></view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="button" @click="Utils.stopMultiClick(submit)">注册</button>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				wx_info:{},
				is_show:false,
				submitData:{
					phone:'',
					password:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			submit() {
				const self = this;
				
				uni.setStorageSync('canClick', false);
				const pass = self.$Utils.checkComplete(self.submitData);
				console.log('pass', pass);
				console.log('self.submitData',self.submitData)
				
				if (pass) {	
						self.register();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全注册信息', 'none')
				};
			},
			
			register() {
				const self = this;
				const postData = {};
				postData.data = {
					phone:self.submitData.phone,
					password:self.submitData.password
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						self.$Utils.showToast('注册成功', 'none', 1000)
						setTimeout(function() {
							uni.navigateBack({
								delta:1
							})
						}, 1000);
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.register(postData, callback);
			},
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
