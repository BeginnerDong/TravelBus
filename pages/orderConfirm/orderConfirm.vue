<template>
	<view>
		
		<view class="mglr4 pdt15">
			
			<view class="pdlr4 pdt15 pdb15 whiteBj">
				<view class="color6 fs13 mgb10">订单描述</view>
				<view class="">[西安1牛背梁] 国家级森林公园，世界生物圈保护区！仅298元=2大人1小抢牛背梁套票~享含双床房1晚+门票2张+双早+免费停车！</view>
			</view>
			
			<view class="radius10 whiteBj mgt15 editLine">
				<view class="item flexRowBetween">
					<view>时间选择：</view>
					<view><image class="arrowR" src="../../static/images/home-icon1.png" mode=""></image></view>
				</view>
				<view class="item flex">
					<view class="ll">购买数量：</view>
					<view class="rr flex">
						<view class="numBox flex">
							<view @click="counter('-')">-</view>
							<view class="num pubColor">{{count}}</view>
							<view @click="counter('+')">+</view>
						</view>
					</view>
				</view>
				<view class="item flex">
					<view class="ll">留言：</view>
					<view class="rr flex">
						<input type="text" value="" placeholder="请输入留言" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
			
			<view class="radius10 whiteBj mgt15 editLine">
				<view class="mglr4 pdt15 pdb5 fs13 color6">订单联系人</view>
				
				<view class="item flex">
					<view class="ll">姓名：</view>
					<view class="rr flex">
						<input type="text" value="张丹" placeholder="请输入姓名" placeholder-class="placeholder" />
					</view>
				</view>
				<view class="item flex">
					<view class="ll">手机：</view>
					<view class="rr flex">
						<input type="text" value="" placeholder="请输入手机号" placeholder-class="placeholder" />
					</view>
				</view>
			</view>
		</view>
		
		<view class="xqbotomBar">
			<view class="mgl15 flex">
				总计<view class="price">66.8</view>
			</view>
			<view class="payBtn"  @click="Router.navigateTo({route:{path:'/pages/orderPay/orderPay'}})">提交订单</view>
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
				count:1
			}
		},
		onLoad() {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
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
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 140rpx;background: #F5F5F5;}
	.editLine .item{padding: 30rpx 4%;}
	
	/* 数量加减 */
	.numBox{border:1px solid #eee; border-radius: 8rpx;}
	.numBox view{ width: 60rpx; height: 44rpx;color: #999; font-size:32rpx ; text-align: center; line-height: 44rpx;}
	.numBox view.num{ width: 80rpx; color: #333; font-size: 26rpx;border-left:1px solid #eee;border-right:1px solid #eee; color: #0591fa;}
</style>
