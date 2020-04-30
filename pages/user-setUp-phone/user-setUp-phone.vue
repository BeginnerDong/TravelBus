<template>
	<view>
		
		<view class="pdlr4 whiteBj" style="padding-top: 40rpx;">
			<view class="flexRowBetween pdtb10">
				<view @click="prev()"><image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL2.png" mode=""></image></view>
				<view class="okBtn fs12 center white pubBj" @click="$Utils.stopMultiClick(userInfoUpdate)">确定</view>
			</view>
		</view>
		<view class="pdlr4 mgt15">
			<view class="pdb10" style="border-bottom: 1px solid #ddd;">
				<input type="text" v-model="submitData.phone" placeholder="请输入联系方式" placeholder-class="placeholder">
			</view>
			<view class="pdtb10 fs12 color9">填写手机号方便使用。</view>
		</view>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				is_show:false,
				submitData:{
					phone:''
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.type){
				self.type=options.type
			};
			self.submitData.phone = options.name
			//self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			prev(){
				const self = this;
				uni.navigateBack({
					delta:1
				})
			},
			
			userInfoUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.submitData.name==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('昵称不能为空', 'none', 1000)
					return
				};
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
						uni.navigateBack({
							delta:1
						})
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
		}
	};
</script>

<style>
	page{background: #F5F5F5;}
	
	.okBtn{width: 45px;line-height: 25px;border-radius: 15px;}    
	
	.placeholder{font-size: 26rpx;}
</style>
