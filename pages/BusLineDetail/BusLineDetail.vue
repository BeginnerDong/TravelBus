<template>
	<view>
		
		<view class="flexRowBetween top-title fs12 whiteBj">
			<view class="ll color6">
				<view class="flex">
					<view>培华学院</view>
					<view><image class="fxArrow" src="../../static/images/line-icon1.png" mode=""></image></view>
					
					<view>城西客运站</view>
				</view>
				<view class="flex">
					<view class="mgr20">首06:00</view>
					<view>末22:00</view>
				</view>
			</view>
			<view class="rr flexColumn">
				<view><image class="switchIcon" src="../../static/images/line-icon.png" mode=""></image></view>
				<view>换向</view>
			</view>
		</view>
		
		<view class="mglr4 pdtb15 whiteBj radius10 mgt15">
			<view class="pdlr4 flexColumn pubColor">
				<view>预计时间<span class="fs24 ftw mgl10">8</span>分</view>
				<view class="mgt5 fs12">2站/1.6千米</view>
			</view>
		</view>
		
		<view class="stationLis">
			<scroll-view class="scroll-item" scroll-x>
				<view class="mglr4" style="display: inline-block">
					<view class="flex" style="align-items: flex-start;">
						<view class="line" v-for="(item,index) in lineData" :key="index" :class="curr==index?'on':''">
							<view class="Gps">
								<image class="carIcon" v-show="item.carOn" :src="curr==index?'../../static/images/line-icon2.1.png':'../../static/images/line-icon2.png'" mode=""></image>
								<image class="currIcon" :src="curr==index?'../../static/images/line-icon3.png':''" mode=""></image>
							</view>
							<view class="xianBJ"><image class="arrow" src="../../static/images/line-icon4.png" mode=""></image></view>
							<view class="infor center">
								<view class="num fs12">{{item.num}}</view>
								<view class="name">{{item.title}}</view>
							</view>
						</view>
					</view>
				</view>
			</scroll-view>
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
				curr:3,
				lineData:[
					{carOn:true,num:1,title:'锦业一路丈八八路口'},
					{carOn:false,num:2,title:'锦业路丈八八路口'},
					{carOn:true,num:3,title:'锦业路丈八七路口'},
					{carOn:false,num:4,title:'锦业路西三环口'},
					{carOn:true,num:5,title:'锦业路丈八六路口'},
					{carOn:true,num:6,title:'锦业路丈八五路口'},
					{carOn:false,num:6,title:'锦业路丈八四路口'},
					{carOn:false,num:7,title:'锦业路丈八三路口'},
					{carOn:true,num:8,title:'唐延路锦业路口'},
					{carOn:false,num:8,title:'丈八东路唐延路口'},
					{carOn:false,num:8,title:'唐延路科技七路口'},
					{carOn:true,num:8,title:'科技六路中段'}
				]
			}
		},
		
		onLoad(options) {
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
	
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	.top-title{padding: 30rpx 4%;}
	.top-title .ll{width: 80%;}
	.top-title .rr{width: 15%;}
	
	.fxArrow{width: 34rpx;height: 12rpx; display: block;margin: 0 20rpx;}
	.switchIcon{width: 34rpx;height: 37rpx; display: block;}
	
	.stationLis{margin-top: 40rpx;}
	.scroll-item{white-space: nowrap;position: relative;}
	.scroll-item .line{padding:60rpx 0;}
	.scroll-item .line.on{color: #ff2626;}
	.scroll-item .line image{width: 100%;height: 100%; display: block;}
	.scroll-item .line .Gps{width: 54rpx;height: 20rpx;margin-left: 10rpx;position: relative;}
	.scroll-item .line .Gps .currIcon{width: 30rpx;height: 35rpx;display: block;margin: 0 auto;position: absolute;left: 50%;transform: translate(-50%,-50%);}
	
	.scroll-item .line .xianBJ{padding:4rpx 80rpx 4rpx 30rpx;background: #15bcaa;margin: 10rpx 0;}
	.scroll-item .line .arrow{width: 16rpx;height: 12rpx;}
	.scroll-item .line .infor{width: 36rpx;margin-left: 20rpx;}
	.scroll-item .line .name{word-wrap: break-word;width: 36rpx;margin: 0 auto;line-height: 32rpx;white-space: initial;}
	.scroll-item .line:first-child .xianBJ{border-radius: 10rpx 0 0 10rpx;}
	.scroll-item .line:last-child .xianBJ{border-radius: 0 10rpx 10rpx 0;padding-right: 40rpx;}
</style>

