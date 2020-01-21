<template>
	<view>
		<!-- 搜索 -->
		<view class="seachbox flexRowBetween whiteBj borderB1"> 
			<view class="flex rr">
				<button class="seachBtn" type="button"></button>
				<view class="input">
					<input type="text" name="" value="" placeholder="搜索公交线路/车站" placeholder-class="placeholder" />
				</view>
			</view>
			<view class="fs15 pubColor" @click="Router.navigateTo({route:{path:'/pages/seachResult/seachResult'}})">查询</view>
		</view>
		
		<view class="mglr4 mgt15">
			<view class="fs14 pdb10">搜索记录</view>
			<view class="notes radius10 fs13">
				<view class="item flex" v-for="(item,index) in notesData" :key="index">
					<view><image class="CarIcon" src="../../static/images/search-icon.png" mode=""></image></view>
					<view>12路</view>
				</view>
				
				<view class="flexCenter pdt15 color6">
					<view @click="popupShow">清除搜索记录</view>
				</view>
			</view>
		</view>
		
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb10 fs15">提示</view>
			<view class="fs13 color6 pdb20 borderB1">确定清空搜索记录吗？</view>
			<view class="flex tip-button">
				<view class="item"  @click="popupShow">取消</view>
				<view class="item pubColor">确定</view>
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
	
	
	
	/* 删除弹框 */
	.popupShow{ width: 80%;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%); z-index: 50;box-sizing: border-box;}
	.tip-button .item{width: 50%;box-sizing: border-box; line-height: 100rpx;}
	.tip-button .item:first-child{border-right:1px solid #eee;}
</style>

