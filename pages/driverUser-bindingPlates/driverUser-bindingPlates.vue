<template>
	<view>
		<view class="myRowBetween whiteBj pdlr4">
			<view class="item flexRowBetween">
				<view class="ll">绑定车牌</view>
				<view class="rr color6">
					<input type="text" value="" placeholder="请输入车牌" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll">始发站-终点站</view>
				<view class="rr color6">
					<view class="changeBtn flexCenter white fs11"><image class="icon" src="../../static/images/binding-plates-icon.png" mode=""></image>换向</view>
				</view>
			</view>
		</view>
		
		<view style="margin-top: 200rpx;">
			<view class="submitbtn">
				<button class="btn" type="button">保存</button>
			</view>
			
			<view class="submitbtn mgt15">
				<button class="btn" type="button">编辑</button>
			</view>
			
			<view class="editMsg flexRowBetween pdt15">
				<view class="submitbtn">
					<button class="deltBtn Wbtn" type="button" @click="popupShow">删除</button>
				</view>
				<view class="submitbtn">
					<button class="Wbtn" type="button">修改</button>
				</view>
			</view>
		</view>
		
		<view class="black-bj" v-show="is_show"></view>
		<view class="popupShow center whiteBj radius10 pdt20" v-show="is_popupShow">
			<view class="tip-title mgb15 fs15">提示：</view>
			<view class="fs13 color6 pdb20 borderB1">确定删除这条信息吗？</view>
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
				score:'',
				wx_info:{},
				is_popupShow:false
			}
		},
		onLoad() {
			const self = this;
			//self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			},
			prev(){
				const self = this;
				self.$router.go(-1)
			}
		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	.changeBtn{width: 120rpx;height: 60rpx;border-radius: 40rpx;background: #0591fa;}
	.changeBtn .icon{width: 30rpx;height: 24rpx;margin-right: 6rpx;}
	
	.editMsg{width: 80%; margin: 0 auto;}
	.editMsg .submitbtn{width: 46%;}
	.editMsg .submitbtn .deltBtn{background: #fff;color: #0591FA;border: 1px solid #0591FA;}
	
	/* 删除弹框 */
	.popupShow{ width: 80%;position: fixed; top: 50%;left: 50%;transform: translate(-50%,-50%); z-index: 50;box-sizing: border-box;}
	.tip-button .item{width: 50%;box-sizing: border-box; line-height: 100rpx;}
	.tip-button .item:first-child{border-right:1px solid #eee;}
</style>
