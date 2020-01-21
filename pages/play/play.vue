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
				<view class="item" v-for="(item,index) in playData" :key="index" @click="Router.navigateTo({route:{path:'/pages/playDetail/playDetail'}})">
					<view class="pic"><image src="../../static/images/swim-around-img.png" mode=""></image></view>
					<view class="infor">
						<view class="avoidOverflow">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！</view>
						<view class="pdt5 price ftw fs15">129</view>
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
				showView: false,
				wx_info:{},
				is_show:false,
				playData:[{},{},{},{}]
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
