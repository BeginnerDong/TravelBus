<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="flexRowBetween pdtb10">
				<view @click="prev()"><image class="arrowR backIcon" style="margin-left: 0;" src="../../static/images/arrowL2.png" mode=""></image></view>
				<view class="okBtn fs12 center white pubBj">确定</view>
			</view>
		</view>
		<view class="pdlr4 mgt15">
			<view class="pdb10" style="border-bottom: 1px solid #ddd;">
				<input type="text" value="雨果果" placeholder="请输入" placeholder-class="placeholder">
			</view>
			<view class="pdtb10 fs12 color9">好名字可以让你的朋友容易记住你。</view>
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
				is_show:false
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
		}
	};
</script>

<style>
	page{background: #F5F5F5;}
	
	.okBtn{width: 45px;line-height: 25px;border-radius: 15px;}    
	
	.placeholder{font-size: 26rpx;}
</style>
