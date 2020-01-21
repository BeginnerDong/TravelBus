<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="ByCarinfor editLine boxShaow" v-for="(item,index) in byCarData" :key="index">
				<view class="fs12 color9">提交时间：2020.01.21</view>
				<view class="item flexRowBetween">
					<view class="ll">上车地点</view>
					<view class="rr">西安高新大都会</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">下车地点</view>
					<view class="rr">兵马俑</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">出发时间</view>
					<view class="rr">2020年1月22日8点</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">人数</view>
					<view class="rr">29人</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">客服电话</view>
					<view class="rr flexRowBetween">
						<view>15623568988</view>
						<view><image class="phoneIcon" src="../../static/images/my-charter-icon.png" mode=""></image></view>
					</view>
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
				byCarData:[{},{},{}]
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
	
	page{padding-bottom: 160rpx;background: #F5F5F5;}
	
	.ByCarinfor{background: #fff;border-radius: 10rpx;padding: 30rpx 4%;margin-bottom: 30rpx;}
	
	.editLine .item{padding:20rpx 0 0 0;border-bottom: 0;}
	.editLine .item .rr{color: #666;font-size: 26rpx;}
	.phoneIcon{width: 38rpx;height: 38rpx; display: block;}
</style>
