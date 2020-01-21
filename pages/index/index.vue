<template>
	<view>
		
		<view class="pdlr4 whiteBj pdt15 pdb15">
			<view class="flex mgb10"><view class="mgr5 ftw fs15">西安</view><image style="width: 15rpx;height: 10rpx;" src="../../static/images/home-icon01.png" mode=""></image></view>
			<view class="seachBox flex color9 fs12" @click="Router.navigateTo({route:{path:'/pages/seach/seach'}})">
				<image class="icon" src="../../static/images/home-icon.png" mode=""></image>
				<view>搜索公交线路、车站</view>
			</view>
		</view>
		<view class="mglr4 pdt15 busLine">
			<view class="item" v-for="(ptem,index) in busLineData" :key="index">
				<view class="infor">
					<view class="flexRowBetween" @click="Router.navigateTo({route:{path:'/pages/BusLineDetail/BusLineDetail'}})">
						<view class="title">高新9号线</view>
						<view><image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
					</view>
					<view class="allList pdt15 flexRowBetween">
						<view class="cont">
							<view class="line">
								<view class="left">始发</view>
								<view class="dian"></view>
								<view class="right">玫瑰大楼</view>
							</view>
							<view class="line">
								<view class="left">途径</view>
								<view class="dian"></view>
								<view class="right">高新一号线</view>
							</view>
							<!-- 展示内容 -->
							<view class="line" v-for="(item,index) in lineData" :key="index" v-show="is_lineShow">
								<view class="left"></view>
								<view class="dian"></view>
								<view class="right">{{item}}</view>
							</view>
							
							<!-- 省略内容 -->
							<view class="omit" v-show="!is_lineShow">
								<view class="dd"><span></span></view>
								<view class="dd"><span></span></view>
								<view class="dd"><span></span></view>
							</view>
							
							<view class="line">
								<view class="left">终点</view>
								<view class="dian"></view>
								<view class="right">协同创新港(南门)</view>
							</view>
						</view>
						<view class="rrOpen fs12 pubColor" @click="changeLine">
							<view v-show="is_lineShow">收起<image class="arrow" src="../../static/images/home-icon2.png" mode=""></image></view>
							<view v-show="!is_lineShow">展开<image class="arrow" src="../../static/images/home-icon2.png" mode=""></image></view>
						</view>
					</view>
				</view>
				<view class="borderB1"></view>
				<view class="infor flex">
					<view class="fs10 color9">始发时间：</view>
					<view class="fs15 ftw">07:00</view>
				</view>
			</view>
		</view>
		
		<!--底部tab键-->
		<view class="navbar">
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/index/index'}})">
				<view class="nav_img">
					<image src="../../static/images/nabar1-a.png" />
				</view>
				<view class="text this-text">首页</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/play/play'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">周边游</view>
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
				busLineData:[{},{},{}],
				lineData:['融发心园','五桥新村','西部大道怡园路口','怡园路'],
				is_lineShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeLine(){
				const self = this;
				self.is_lineShow = !self.is_lineShow
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
	@import "../../assets/style/navbar.css";
	/* @import "../../assets/style/product.css"; */
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.seachBox{border-radius: 30rpx;background: #f5f5f5;padding:10rpx 80rpx 10rpx 30rpx;position: relative;box-sizing: border-box;}
	.seachBox input{width: 100%;min-height: auto;line-height: 40rpx;height: 40rpx;display: block;font-size: 26rpx;}
	.seachBox .icon{width: 30rpx;height: 30rpx;margin-right: 10rpx;}
	.seachBox .rr{width: 60rpx;height: 40rpx;position: absolute; right: 10rpx;top: 50%;transform: translateY(-50%);}	
	.seachBox .btn{width:100%;height: 100%;background: url(../../static/images/home-icon.png) no-repeat center center /30rpx 30rpx;}
	
	.busLine .item{background: #fff;border-radius: 10rpx; margin-bottom: 30rpx;}
	.busLine .item .infor{padding: 30rpx 4%;}
	.busLine .item .title{font-size: 30rpx;font-weight: bold;}
	
	.allList .cont{width: 85%;}
	.allList .rrOpen{width:12%;}
	.allList .rrOpen .arrow{width: 18rpx;height: 10rpx;margin-left: 10rpx;}
	.allList .line{display: flex;flex-direction: row;overflow: hidden;position: relative;padding-bottom:20rpx;box-sizing: border-box;}
	.allList .line .left{width: 70rpx;line-height: 40rpx;text-align: right;padding-right: 20rpx;font-size: 22rpx;color: #666;}
	.allList .line .dian{width: 18rpx;height: 18rpx;border-radius: 50%;background: #15bcaa;position: relative;top: 10rpx;}
	.allList .line .dian::after{content: ''; position: absolute;left: 8rpx;top: 100%;width: 1px;height: 100vh;background: #eee;}
	.allList .line .dian::before{content: ''; position: absolute;left: 8rpx;bottom: 100%;width: 1px;height: 100vh;background: #eee;}
	.allList .line .right{padding-left: 20rpx;font-size: 26rpx;}
	.allList .omit{margin-left:98rpx;border-left: 1px solid #eee;padding-bottom:20rpx;}
	.allList .omit .dd{padding: 10rpx 0;}
	.allList .omit .dd span{width: 10rpx;height: 10rpx;background: #eee;border-radius: 50%; display: block;margin-left: 30rpx;}
	.allList .line:first-child .dian::before{display: none;}
	.allList .line:last-child .dian::after{display: none;}
	.allList .line:last-child .dian{background: #ff2626;}
</style>
