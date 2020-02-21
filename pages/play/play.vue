<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox whiteBj borderB1"> 
			<view class="flex rr">
				<view class="seachBtn"><button class="btn" type="submint"></button></view>
				
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索公交线路/车站" placeholder-class="placeholder" />
				</view>
			</view>
		</view>
		
		<view class="mglr4 pdt15">
			<view class="playList">
				<view class="item" v-for="(item,index) in mainData" :key="index" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/playDetail/playDetail?id='+$event.currentTarget.dataset.id}})">
					<view class="pic"><image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image></view>
					<view class="infor">
						<view class="avoidOverflow">{{item.title}}</view>
						<view class="pdt5 price ftw fs15">{{item.price}}</view>
					</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1.png" />
				</view>
				<view class="text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/play/play'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2-a.png" />
				</view>
				<view class="text this-text">周边游</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3.png" />
				</view>
				<view class="text">智能包车</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/user/user'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar4.png" />
				</view>
				<view class="text">我的</view>
			</view>
		</view>
		<!--底部tab键 end-->
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				
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
				};
				postData.getBefore = {
					caseData: {
						tableName: 'Label',
						searchItem: {
							title: ['in', ['周边游']],
						},
						middleKey: 'category_id',
						key: 'id',
						condition: 'in',
					},
				};
				postData.order = {
					listorder:'desc'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					self.$Utils.finishFunc('getMainData');
					console.log('self.mainData', self.mainData)
				};
				self.$apis.productGet(postData, callback);
			},
		}
	};
</script>

<style>
	@import "../../assets/style/navbar.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.seachbox{padding: 30rpx 4%;box-sizing: border-box;}
	.seachbox .Gps{max-width: 140rpx;}
	.seachbox .Licon{width: 20rpx; height: 26rpx; display: block;}
	.seachbox .rr{background:#F5F5F5;border-radius: 40rpx;overflow: hidden;padding: 0 20rpx;box-sizing: border-box;}
	.seachbox .rr .input{width:80%;}
	.seachbox .rr .input input{width: 100%;line-height: 60rpx;padding: 0 20rpx 0 0px;box-sizing: border-box;display: block;border: none;font-size: 26rpx;}
	.seachbox .rr .seachBtn{width: 10%;height: 60rpx; display: block;margin-right: 20rpx;}
	.seachbox .rr .seachBtn .btn{width: 100%;height: 100%;background: url(../../static/images/home-icon.png) no-repeat center center/30rpx 30rpx;border: none;}
	
	.playList .item{border-radius: 10rpx;overflow: hidden;background: #fff;margin-bottom: 30rpx;}
	.playList .item .pic{width: 100%;height: 400rpx;}
	.playList .item .pic image{width: 100%;height: 100%; display: block;}
	.playList .item .infor{padding: 20rpx 4%;}
	
	
</style>
