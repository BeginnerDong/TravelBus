<template>
	<view>
		
		<view class="mglr4 flexColumn topPhone">
			<view><image class="phone-icon" src="../../static/images/contact-icon.png" mode=""></image></view>
			<view class="fs16 mgt15">{{object.phone}}</view>
		</view>
		<view class="map"> <map style="width: 100%; height: 934rpx;" :latitude="object.latitude" :longitude="object.longitude" :markers="covers">
                </map></view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				object:{},
				covers: [{
					latitude: '',
					longitude: '',
					iconPath: '../../static/images/line-icon3.png'
				}]
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
			self.object = uni.getStorageSync('userInfo').thirdApp;
			self.covers[0].latitude = uni.getStorageSync('userInfo').thirdApp.latitude;
			self.covers[0].longitude = uni.getStorageSync('userInfo').thirdApp.longitude;
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'客服'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.thirdappGet(postData, callback);
			},
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
