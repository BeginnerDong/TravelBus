<template>
	<view>
		
		<view class="mglr4 flexColumn topPhone">
			<view><image class="phone-icon" src="../../static/images/contact-icon.png" mode=""></image></view>
			<view class="fs16 mgt15">15623568912</view>
		</view>
		<view class="map"><image src="../../static/images/map.png" mode=""></image></view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				showView: false,
				wx_info:{},
				is_show:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
		}
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.topPhone{padding: 80rpx 4%;}
	.phone-icon{width: 69rpx;height: 56rpx;display: block;}
	.map{width: 100%;height: 934rpx;}
	.map image{width: 100%;height: 100%;display: block;}
</style>
