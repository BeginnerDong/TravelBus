<template>
	<view>
		
		<view class="mglr4 mgt15">
			<view class="notes radius10 fs13">
				<view class="item flex" v-for="(item,index) in notesData" :key="index"  @click="Router.navigateTo({route:{path:'/pages/BusLineDetail/BusLineDetail'}})">
					<view><image class="CarIcon" src="../../static/images/search-icon.png" mode=""></image></view>
					<view>12路</view>
				</view>
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
				wx_info:{},
				is_show:false,
				notesData:[{},{},{}],
				is_popupShow:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			},
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
	/* @import "../../assets/style/seach.css"; */
	
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	
	.notes{padding: 30rpx 4%;background: #fff;}
	.notes .item{padding-bottom: 30rpx;}
	.notes .item:last-child{padding-bottom: 0;}
	.notes .item .CarIcon{width: 26rpx;height: 26rpx;margin-right:20rpx;}
	
</style>

