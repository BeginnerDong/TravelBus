<template>
	<view>
		
		<view class="mglr4 pdt15">
			<view class="ByCarinfor editLine boxShaow" v-for="(item,index) in mainData" :key="index">
				<view class="fs12 color9">提交时间：{{item.create_time}}</view>
				<view class="item flexRowBetween">
					<view class="ll">上车地点</view>
					<view class="rr">{{item.title}}</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">下车地点</view>
					<view class="rr">{{item.description}}</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">出发时间</view>
					<view class="rr">{{item.keywords}}</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">人数</view>
					<view class="rr">{{item.score}}人</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll">客服电话</view>
					<view class="rr flexRowBetween">
						<view>{{item.phone}}</view>
						<view @click="tel()"><image class="phoneIcon" src="../../static/images/my-charter-icon.png" mode=""></image></view>
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
				byCarData:[{},{},{}],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					type:2
				};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.getLabelData()
					console.log('self.mainData', self.mainData)
				};
				self.$apis.messageGet(postData, callback);
			},
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'客服'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0];
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].phone = self.labelData.description
						}
					}
					console.log('self.labelData', self.labelData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			tel(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.labelData.description //仅为示例
				});
			},
			
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
