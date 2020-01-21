<template>
	<view>
		<view class="pdlr4 whiteBj">
			<view class="banner-box">
				<view class="banner pdtb15">
					<swiper class="swiper-box" indicator-dots="true" autoplay="true" interval="3000" duration="1000" indicator-active-color="#fff">
						<block v-for="(item,index) in labelData" :key="index">
							<swiper-item class="swiper-item">
								<image :src="item" class="slide-image" />
							</swiper-item>
						</block>
					</swiper>
				</view>
			</view>
			<view class="noticeLis flex pdb10">
				<view class="Licon mgr10">
					<image src="../../static/images/charter-icon.png" mode=""></image>
				</view>
				<view class="rr fs13">
					<swiper vertical="true" autoplay="true" circular="true" interval="3000">
						<swiper-item v-for="(item,index) in msg" :key="index">
							<navigator class="avoidOverflow">{{item}}</navigator>
						</swiper-item>
					</swiper>
				</view>
			</view>
		</view>
		<view class="mglr4 pdt15">
			<view class="ByCarinfor pdb25 whiteBj editLine radius10 boxShaow">
				<view class="seltNav flexRowBetween pdtb15 borderB1">
					<view class="tt flexCenter" :class="curr==1?'on':''" @click="changeCurr('1')"><image class="icon" :src="curr==1?'../../static/images/charter-icon1.png':'../../static/images/charter-icon2.png'" mode=""></image>单程</view>
					<view class="tt flexCenter" :class="curr==2?'on':''" @click="changeCurr('2')">
						<image class="icon" :src="curr==2?'../../static/images/charter-icon1.png':'../../static/images/charter-icon2.png'" mode=""></image>往返</view>
				</view>
				<view class="editLine pdlr4">
					<view class="item flexRowBetween">
						<view class="ll">上车地点</view>
						<view class="rr"><input type="text" placeholder="请输入地点" placeholder-class="placeholder"></view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll">下车地点</view>
						<view class="rr"><input type="text" placeholder="请输入地点" placeholder-class="placeholder"></view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll">出发时间</view>
						<view class="rr"><input type="text" placeholder="请输入时间" placeholder-class="placeholder"></view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll">出行人数</view>
						<view class="rr"><input type="text" placeholder="请输入出行人数" placeholder-class="placeholder"></view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll">电话号码</view>
						<view class="rr"><input type="text" placeholder="请输入电话号码" placeholder-class="placeholder"></view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll">备注</view>
						<view class="rr"><input type="text" placeholder="请输入备注" placeholder-class="placeholder"></view>
					</view>
				</view>
			
				<view class="submitbtn" style="margin-top: 100rpx;">
					<button class="btn" type="submint"  @click="driverMsgShow">提交</button>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="driverMsg flexColumn" v-show="is_driverMsgShow">
			<view class="closebtn" @click="driverMsgShow">×</view>
			<view><image class="carIcon" src="../../static/images/charter-icon3.png" mode=""></image></view>
			<view class="fs12 color6 mgt20 mgb5">客服名称</view>
			<view class="fs15">战歌</view>
			<view class="fs12 color6 mgt20 mgb5">联系方式</view>
			<view class="fs15">15623561254</view>
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
					<image src="../../static/images/nabar2.png" />
				</view>
				<view class="text">周边游</view>
			</view>
			<view class="navbar_item" @click="Router.redirectTo({route:{path:'/pages/car/car'}})" >
				<view class="nav_img">
					<image src="../../static/images/nabar3-a.png" />
				</view>
				<view class="text this-text">智能包车</view>
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
				labelData: [
					"../../static/images/charter-banner.png",
					"../../static/images/charter-banner.png"
				],
				msg:[
					'通知行业峰会频频亮相，开发者反响热烈通知公告',
					'DCloud完成B2轮融资，uni-app震撼发布热烈通知公告热烈通知公告',
					'价格记录可估计发来电管家就过来开发多久干两份价格付款聊到几个通知公告'
				],
				curr:1,
				playData:[{},{},{},{}],
				is_driverMsgShow:false
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeCurr(curr){
				const self = this;
				if(curr!=self.curr){
					self.curr = curr
				}
			},
			driverMsgShow(){
				const self = this;
				self.is_show = !self.is_show
				self.is_driverMsgShow = !self.is_driverMsgShow
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
	@import "../../assets/style/editInfor.css";
	
	page{padding-bottom: 160rpx;background: #F5F5F5;}
	.swiper-box {height: 340rpx;border-radius: 10rpx;box-sizing: border-box;overflow: hidden;}
	.swiper-box swiper-item {width: 100%;}
	.swiper-box image {width: 100%;height: 100%;}
	
	/* 公告滚动 */
	.noticeLis{overflow: hidden;}
	.noticeLis .Licon{width: 34rpx;height: 34rpx;}
	.noticeLis .Licon image{width: 100%;height: 100%;display: block;}
	.noticeLis .rr{width: 100%;height: 40rpx;}
	.noticeLis .rr swiper{height:40rpx;width: 100%;}
	
	
	.seltNav .tt{width: 50%;box-sizing: border-box;}
	.seltNav .tt:first-child{border-right: 1px solid #eee;}
	.seltNav .tt .icon{width: 34rpx;height: 34rpx;margin-right: 20rpx;}
	
	
	.driverMsg{width:80%;background: #fff;border-radius: 10rpx;padding:120rpx 4%;box-sizing: border-box;position: fixed;top: 50%;left: 50%;transform: translate(-50%,-50%);z-index: 45;}
	.driverMsg .carIcon{width:300rpx;height: 109rpx; display: block;margin: 0 auto;}
	
	
</style>
