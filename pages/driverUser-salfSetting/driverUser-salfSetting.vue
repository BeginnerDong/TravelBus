<template>
	<view>
		
		<view class="pageTit center pr flexRowBetween pdlr4 whiteBj borderB1" style="line-height: 44px;font-size: 16px;padding-top: 40rpx;">
			<view @click="prev()"><image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL2.png" mode=""></image></view>
			<view>安全设置</view>
			<view><view class="fs12 pubColor flexEnd" 
			@click="Router.navigateTo({route:{path:'/pages/driverUser-changePswd/driverUser-changePswd'}})">修改密码</view></view>
		</view>
		<view class="myRowBetween whiteBj pdlr4">
			<view class="item flexRowBetween">
				<view class="ll">账号</view>
				<view class="rr color6">{{userData.login_name}}</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">密码</view>
				<view class="rr color6">{{userData.password}}</view>
			</view>
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
				wx_info:{},
				userData:{}
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserData'], self);
		},
		methods: {
			prev(){
				const self = this;
				uni.navigateBack({
					delta:1
				})
			},
			
			getUserData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('driverInfo').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userData = res.info.data[0];	
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserData');
				};
				self.$apis.userGet(postData, callback);
			},
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;}
	.pageTit>view{width: 50%;}
</style>
