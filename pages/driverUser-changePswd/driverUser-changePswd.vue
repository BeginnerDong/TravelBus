<template>
	<view>
		
		<view class="pdlr4 pswdCont">
			<view class="item">
				<view class="tit">原始密码</view>
				<view class="input">
					<input type="password" v-model="submitData.o_password" placeholder="请输入原密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item">
				<view class="tit">新密码</view>
				<view class="input">
					<input type="password" v-model="submitData.n_password" placeholder="请输入新密码" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item">
				<view class="tit">确认新密码</view>
				<view class="input">
					<input type="password" v-model="submitData.passwordCopy" placeholder="请确认新密码" placeholder-class="placeholder">
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 200rpx;">
			<button class="btn" type="submit" @click="$Utils.stopMultiClick(submit)">确认</button>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router: this.$Router,
				Utils: this.$Utils,
				index: 0,
				is_show: false,
				type: '',
				mode: '',
				submitData: {
					o_password: '',
					n_password: '',
					passwordCopy: ''
				}
			}
		},
		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick', true);
		},

		methods: {



			submit() {
				const self = this;
				uni.setStorageSync('canClick', false);
				var newObject = self.$Utils.cloneForm(self.submitData);

				const pass = self.$Utils.checkComplete(newObject);
				console.log('pass', pass);
				console.log('self.submitData', self.submitData)
				if (pass) {
					if (self.submitData.o_password != uni.getStorageSync('driverInfo').password) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('原密码错误', 'none');
						return
					};
					if (self.submitData.n_password != self.submitData.passwordCopy) {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast('两次输入密码不一致', 'none');
						return
					};
					self.passwordUpdate();
				} else {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请补全信息', 'none')
				};
			},

			passwordUpdate() {
				const self = this;
				const postData = {};
				
				postData.tokenFuncName = 'getDriverToken';
				postData.data = {
					password:self.submitData.n_password
				};
				const callback = (data) => {
					if (data.solely_code == 100000) {
						self.$Utils.showToast('修改成功,请重新登陆', 'none');
						setTimeout(function() {
							uni.removeStorageSync('driverToken');
							uni.removeStorageSync('driverInfo');
							uni.reLaunch({
								url: '/pages/user/user'
							});
						}, 800);
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}
				};
				self.$apis.userUpdate(postData, callback);
			},

		},
	};
</script>

<style>
	page{padding-bottom:60rpx;}
	.pswdCont .item{margin-top: 50rpx;}
	.pswdCont .tit{margin-bottom: 20rpx;}
	.pswdCont .item input{width: 100%;height:80rpx;line-height: 40rpx;padding: 20rpx;box-sizing: border-box;font-size: 26rpx;background: #F5F5F5;border-radius: 10rpx;}
</style>
