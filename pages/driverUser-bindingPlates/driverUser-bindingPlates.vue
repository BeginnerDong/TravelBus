<template>
	<view>
		<view class="myRowBetween whiteBj pdlr4">
			<view class="item flexRowBetween">
				<view class="ll">绑定车牌</view>
				<view class="rr color6">
					<input type="text" v-model="no"  placeholder="请输入车牌" placeholder-class="placeholder">
				</view>
			</view>
			<view class="item flexRowBetween">
				<view class="ll" v-if="stopData.length>0">{{stopData[0].name}}-{{stopData[stopData.length-1].name}}</view>
				<view class="ll" v-else>始发站-终点站</view>
				<view class="rr color6"  @click="$Utils.stopMultiClick(busUpdate)" >
					<view class="changeBtn flexCenter white fs11"><image class="icon" src="../../static/images/binding-plates-icon.png" mode=""></image>换向</view>
				</view>
			</view>
		</view>
		
		<view style="margin-top: 200rpx;">
			<view class="submitbtn">
				<button class="btn" type="button" @click="getBusData">保存</button>
			</view>
			
			<!-- <view class="submitbtn mgt15">
				<button class="btn" type="button">编辑</button>
			</view>
			
			<view class="editMsg flexRowBetween pdt15">
				<view class="submitbtn">
					<button class="deltBtn Wbtn" type="button" @click="popupShow">删除</button>
				</view>
				<view class="submitbtn">
					<button class="Wbtn" type="button">修改</button>
				</view>
			</view> -->
		</view>
		
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils:this.$Utils,
				showView: false,
				score:'',
				wx_info:{},
				is_popupShow:false,
				no:'',
				userInfoData:{
					
				},
				stopData:[]
			}
		},
		onLoad() {
			const self = this;
			self.$Utils.loadAll(['getUserInfoData'], self);
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('driverInfo').user_no
				};
				postData.getAfter = {
					bus:{
						tableName:'Bus',
						middleKey:'bus_no',
						key:'no',
						searchItem:{
							status:1
						},
						condition:'=',
						
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
							self.no = self.userInfoData.bus_no;
						if(self.userInfoData.line_no!=''){
							self.getStopData()
						}
					};
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getStopData() {
				const self = this;
				self.stopData = [];
				const postData = {};
				//postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					line_no:self.userInfoData.line_no
				};
				if(self.userInfoData.bus[0].direction==1){
					postData.order = {
						listorder:'desc'
					}
				}else if(self.userInfoData.bus[0].direction==2){
					postData.order = {
						listorder:'asc'
					}
				}; 
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.stopData.push.apply(self.stopData,res.info.data);	
					}
					console.log('self.busData', self.busData)
				};
				self.$apis.stopGet(postData, callback);
			},
			
			getBusData() {
				const self = this;
				if (self.no=='') {
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('请输入正确的车牌号', 'none')
					return
				};
				const postData = {};
				//postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					no:self.no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.busData = res.info.data[0];
						self.userInfoUpdate()
					}else{
						self.$Utils.showToast('未找到车辆信息', 'none');
						self.no = '';
					}
					console.log('self.busData', self.busData)
				};
				self.$apis.busGet(postData, callback);
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					user_no: uni.getStorageSync('driverInfo').user_no
				};
				postData.data = {
					bus_no:self.no
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData();
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			busUpdate() {
				const self = this;
				uni.setStorageSync('canClick', false);
				if(self.userInfoData.bus_no==''){
					uni.setStorageSync('canClick', true);
					self.$Utils.showToast('您暂未绑定车牌', 'none', 1000)
					return
				};
				const postData = {};
				postData.tokenFuncName = 'getDriverToken';
				postData.searchItem = {
					no: self.userInfoData.bus_no,
					user_type:2
				};
				postData.data = {
					direction :self.userInfoData.bus[0].direction==0||self.userInfoData.bus[0].direction==1?2:1
				};
				const callback = (data) => {				
					if (data.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						self.getUserInfoData();
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.busUpdate(postData, callback);
			},
			
			popupShow(){
				const self=this;
				self.is_popupShow = !self.is_popupShow;
				self.is_show = !self.is_show;
				
			},
			prev(){
				const self = this;
				uni.navigateBack({
					delta:1
				})
			},
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
