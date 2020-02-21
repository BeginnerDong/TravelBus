<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"> 
			<view class="flex rr">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" v-model="keywords" placeholder="搜索公交线路/车站" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="fs15 pubColor" @click="search">查询</view>
		</view>
		
		<view class="mglr4 mgt20">
			<view class="fs14 pdb10">线路</view>
			<view class="notes radius10 fs13">
				<view class="item flex" v-for="(item,index) in line" :key="index"  :data-id="item.id" 
				@click="Router.navigateTo({route:{path:'/pages/BusLineDetail/BusLineDetail?id='+$event.currentTarget.dataset.id}})">
					<view><image class="CarIcon" src="../../static/images/search-icon.png" mode=""></image></view>
					<view>{{item.name}}</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4 mgt20">
			<view class="fs14 pdb10">车站</view>
			<view class="notes radius10 fs13">
				<view class="item flex" v-for="(item,index) in stop" :key="index" 
				@click="Router.navigateTo({route:{path:'/pages/linePlan/linePlan'}})">
					<view><image class="CarIcon" src="../../static/images/search-icon1.png" mode=""></image></view>
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
				line:[],
				stop:[],
				keywords:''
			}
		},
		
		onLoad(options) {
			const self = this;
			if(options.keywords){
				self.keywords = options.keywords;
			}
			self.$Utils.loadAll(['getMainData'], self);
			// self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			search(){
				const self =  this;
				self.getMainData(true)
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
				};
				const postData = {};
				postData.tokenFuncName='getUserToken';
				postData.data = {
					search:self.keywords
				};
				const callback = (res) => {
					if (res.solely_code ==100000) {
						//self.mainData.push.apply(self.mainData,res.info.data)
						if(res.info.line.data&&res.info.line.data.length>0){
							self.line = res.info.line.data;
						}
						if(res.info.stop.data&&res.info.stop.data.length>0){
							self.stop = res.info.stop.data;
						}
						
					}
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.searchLine(postData, callback);
			},
		}
	};
</script>

<style>
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.seachbox{padding: 30rpx 4%;box-sizing: border-box;}
	.seachbox .Gps{max-width: 140rpx;}
	.seachbox .Licon{width: 20rpx; height: 26rpx; display: block;}
	.seachbox .rr{width: 85%;background:#F5F5F5;border-radius: 40rpx;overflow: hidden;padding: 0 20rpx;box-sizing: border-box;}
	.seachbox .rr .input{width:90%;}
	.seachbox .rr .input input{width: 100%;line-height: 60rpx;padding: 0 20rpx 0 0px;box-sizing: border-box;display: block;border: none;font-size: 26rpx;}
	.seachbox .rr .seachBtn{width: 10%;height: 60rpx; display: block;background: url(../../static/images/home-icon.png) no-repeat center center/30rpx 30rpx;border: none;}
	.seachbox .delt{width: 10%; height: 26rpx;}
	.seachbox .delt text{width: 28rpx;height: 28rpx;border-radius: 50%;background: #cfcfcf; color: #fff; line-height: 26rpx;text-align: center;}
	
	.notes{padding: 30rpx 4%;background: #fff;}
	.notes .item{padding-bottom: 30rpx;}
	.notes .item:last-child{padding-bottom: 0;}
	.notes .item .CarIcon{width: 26rpx;height: 26rpx;margin-right:20rpx;}
	

</style>

