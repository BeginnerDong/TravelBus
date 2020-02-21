<template>
	<view>
		
	 	<view class="detailxqBan">
			<image :src="mainData.bannerImg&&mainData.bannerImg[0]?mainData.bannerImg[0].url:''" mode=""></image>
		</view>
		<view class="mglr4 pdtb10">
			<view class="">{{mainData.title}}</view>
			<view class="mgt5 price fs15 ftw">{{mainData.price}}</view>
		</view>
		<view class="f5H5"></view>
		
		<view class="mglr4 pdt15">
			<view class="fs15 ftw mgb15">景点详情</view>
			<view class="xqInfor fs13">
				<view class="cont center">
					<view class="content ql-editor" style="padding:0;"
					v-html="mainData.content">
					</view>
				</view>
			</view>
		</view>
		<view class="xqbotomBar pdlr4 pdt10 pdb10">
			<view class="ite" @click="tel()">
				<view><image src="../../static/images/details-icon.png" mode=""></image></view>
				<view>客服</view>
			</view>
			<view class="submitbtn" style="width: 72%;">
				<button class="Wbtn" type="button"  @click="Utils.stopMultiClick(goBuy)">立即购买</button>
			</view>
		</view>
		
	</view>
	
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				Utils:this.$Utils,
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData','getLabelData'], self);
		},
		
		onShow() {
			const self = this;
			self.orderList = [];
			uni.removeStorageSync('payPro');
		},
		
		methods: {
			
			getLabelData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					title:'客服'
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.labelData = res.info.data[0];
					}
					console.log('self.labelData', self.labelData)
					self.$Utils.finishFunc('getLabelData');
				};
				self.$apis.labelGet(postData, callback);
			},
			
			goBuy(){
				const self = this;
				uni.setStorageSync('canClick',false);
				self.orderList.push(
					{product_id:self.mainData.id,count:1,
					type:1,product:self.mainData},
				);
				uni.setStorageSync('payPro', self.orderList);
				
				self.Router.navigateTo({route:{path:'/pages/orderConfirm/orderConfirm'}})
				
				uni.setStorageSync('canClick',true);
			},
			
			//拨打客服电话
			tel(){
				const self = this;
				uni.makePhoneCall({
					phoneNumber: self.labelData.description //仅为示例
				});
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/detail.css";
	
	page{padding-bottom: 160rpx;}
	
	
</style>
