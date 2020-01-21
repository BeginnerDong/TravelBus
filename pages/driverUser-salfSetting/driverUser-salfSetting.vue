<template>
	<view>
		
		<view class="pageTit center pr flexRowBetween pdlr4 whiteBj borderB1" style="line-height: 44px;font-size: 16px;">
			<view @click="prev()"><image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL2.png" mode=""></image></view>
			<view>安全设置</view>
			<view><view class="fs12 pubColor flexEnd" @click="Router.navigateTo({route:{path:'/pages/driverUser-changePswd/driverUser-changePswd'}})">修改密码</view></view>
		</view>
		<view class="myRowBetween whiteBj pdlr4">
			<view class="item flexRowBetween">
				<view class="ll">账号</view>
				<view class="rr color6">15623567566</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">密码</view>
				<view class="rr color6">hfdjshf123</view>
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
				score:'',
				wx_info:{}
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			prev(){
				const self = this;
				self.$router.go(-1)
			}
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	page{background: #F5F5F5;}
	.pageTit>view{width: 50%;}
</style>
