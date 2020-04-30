<template>
	<view>
		
		<view class="mglr4 mgt15">
			<view class="notes radius10 fs13">
				<view class="item flex" v-for="(item,index) in mainData" :data-id="item.id" 
				:key="index"  @click="Router.navigateTo({route:{path:'/pages/BusLineDetail/BusLineDetail?id='+$event.currentTarget.dataset.id}})">
					<view><image class="CarIcon" src="../../static/images/search-icon.png" mode=""></image></view>
					<view>{{item.name}}</view>
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
				is_popupShow:false,
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			console.log(options)
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id:['in',self.id.split(',')]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					}
					self.$Utils.finishFunc('getMainData');
					console.log('self.mainData', self.mainData)
				};
				self.$apis.busLineGet(postData, callback);
			},
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

