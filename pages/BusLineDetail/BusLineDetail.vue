<template>
	<view>
		
		<view>当前经度:{{melongitude}},当前纬度:{{melatitude}}</view>
		<view>当前车辆经度:{{busInfo.longitude}},当前纬度:{{busInfo.latitude}}</view>
		<view>车辆位置更新时间:{{busInfo.update_time}}</view>
		<map style="width: 100%; height: 500rpx;" id="map"  :latitude="covers[0].latitude" :longitude="covers[0].longitude"
		 :markers="covers">
		</map>
		<button style="color: #fff;margin: 0 auto;background-color: #0066CC;" @click="getCenter">确定位置（当前地图视野中心点）</button>
		<view class="flexRowBetween top-title fs12 whiteBj">
			<view class="ll color6">
				<view class="flex">
					<view>{{line[0]&&line[0].name?line[0].name:''}}</view>
					<view>
						<image class="fxArrow" src="../../static/images/line-icon1.png" mode=""></image>
					</view>

					<view>{{line[line.length-1]&&line[line.length-1].name?line[line.length-1].name:''}}</view>
				</view>
				<view class="flex">
					<view class="mgr20">首{{lineData.start?lineData.start:''}}</view>
					<view>末{{lineData.end?lineData.end:''}}</view>
				</view>
			</view>
			<view class="rr flexColumn" @click="changeDirection">
				<view>
					<image class="switchIcon" src="../../static/images/line-icon.png" mode=""></image>
				</view>
				<view>换向</view>
			</view>
		</view>

		<view class="mglr4 pdtb15 whiteBj radius10 mgt15">
			<view class="pdlr4 flexColumn pubColor" v-if="busInfo.rest_time||busInfo.rest_time==0">
				<view>预计时间<span class="fs24 ftw mgl10">{{busInfo.rest_time||busInfo.rest_time==0?busInfo.rest_time:''}}</span>分</view>
				<view class="mgt5 fs12">{{busInfo.rest_stop||busInfo.rest_stop==0?busInfo.rest_stop:''}}站/{{busInfo.rest_distance||busInfo.rest_distance==0?busInfo.rest_distance:''}}米</view>
			</view>
			<view class="pdlr4 flexColumn pubColor" v-else>
				<view class="mgt5 fs12">该方向暂无运行车辆</view>
			</view>
		</view>
		
		<view class="stationLis">

			<scroll-view class="scroll-item" scroll-x>
				<view class="mglr4" style="display: inline-block">
					<view class="flex" style="align-items: flex-start;">
						<view class="line" v-for="(item,index) in line" :key="index" :class="item.id==busInfo.last_stop?'on':''">
							<view class="Gps">
								<image class="carIcon" :style="busInfo.stop_distance-busInfo.bus_distance<10?'margin-left: 10rpx':'margin-left: 70rpx'"
								 v-if="item.id==busInfo.last_stop" src="../../static/images/line-icon2.1.png" mode=""></image>
								<image class="currIcon" style="margin-left: 10rpx;" :src="nearStop.id==item.id?'../../static/images/line-icon3.png':''"
								 mode=""></image>
							</view>
							<view class="xianBJ">
								<image class="arrow" src="../../static/images/line-icon4.png" mode=""></image>
							</view>
							<view class="infor center">
								<view class="num fs12">{{index+1}}</view>
								<view class="name">{{item.name}}</view>
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
				Router: this.$Router,
				showView: false,
				wx_info: {},
				is_show: false,
				curr: 3,

				direction: 1,
				timeData: [],
				line: [],
				nearStop: {},
				busInfo: {},
				lineData: {},
				test: {
					name: '',
					address: ''
				},
				covers: [{
					label: {
						content: '当前位置',

					},

					latitude: '',
					longitude: '',
					iconPath: '../../static/images/line-icon3.png'
				}],
				melongitude: '',
				melatitude: ''
			}
		},

		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getLineData','getLocation'], self);
			self.getMainData()
		},

		onPullDownRefresh() {
			console.log('refresh');
			const self = this;
			self.getLocation();

		},
		
		onReady() {
			const self = this;
			self.mapContext = uni.createMapContext('map', self).$getAppMap();
			console.log(self.mapContext)
		},

		methods: {

		
			changeDirection() {
				const self = this;
				if (self.direction == 1) {
					self.direction = 2
				} else {
					self.direction = 1
				}
				self.getMainData()
			},


			getCenter: function() {
				const self = this;
				self.mapContext.getCurrentCenter(
					function(state, point) {
						if (0 == state) {
							// 反编码
							var point = new plus.maps.Point(point["longitude"], point["latitude"]);
							plus.maps.Map.reverseGeocode(point, {}, function(event) {
								var address = event.address; // 转换后的地理位置
								var coord = event.coord; // 转换后的坐标信息
								var coordType = event.coordType; // 转换后的坐标系类型
								
								uni.showModal({
									title: "提示",
									content: "确定移动到经度：" + coord.longitude + "纬度:"+coord.latitude+'的位置上？',
									success: function(res) {
										if (res.confirm) {
											self.melongitude = coord.longitude;
											self.melatitude = coord.latitude;
											self.covers[0].longitude = coord.longitude;
											self.covers[0].latitude = coord.latitude;
											self.getMainData()
										} else if (res.cancel) {}
									}
								})
							}, function(e) {
								// console.log("Failed:" + JSON.stringify(e));
								uni.showToast({
									title: '反编码失败' + JSON.stringify(e)
								});
							});
						} else {
							uni.showToast({
								icon: "none",
								title: "获取经纬度失败!" + state
							})
						}
					}
				);
			},

			getLocation() {
				const self = this;
				console.log('res', 222)
				uni.getLocation({
					type: 'gcj02',
					success: function(res) {
					
						self.melongitude = res.longitude;
						self.melatitude = res.latitude;
						self.covers[0].longitude = res.longitude;
						self.covers[0].latitude = res.latitude;
						/* self.melongitude = 108.5010;
						self.melatitude = 34.30012; */
						self.test.address = res.location

						
						self.getMainData()
					},
					fail() {
						uni.showToast({
							title: 'fail'
						});
					}
				});
				self.$Utils.finishFunc('getLocation');
			},

			

			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getUserToken';
				postData.data = {
					line_no: self.id,
					longitude: self.melongitude,
					latitude: self.melatitude,
				};
				postData.data.direction = self.direction;
				console.log('postData',postData)
				const callback = (res) => {
					if (res.solely_code == 100000) {

						self.line = res.info.line;
						self.nearStop = res.info.stop;
						self.busInfo = res.info.bus;

					} else {
						self.$Utils.showToast(res.msg, 'none', 1000)
					}
					console.log('busInfo',self.busInfo)
					setTimeout(function() {
						uni.stopPullDownRefresh();
					}, 1000);
					self.$Utils.finishFunc('getLocation');
				};
				self.$apis.getBus(postData, callback);
			},

			getLineData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.lineData = res.info.data[0];
					}
					self.$Utils.finishFunc('getLineData');

				};
				self.$apis.busLineGet(postData, callback);
			},

		}
	};
</script>

<style>
	page {
		padding-bottom: 60rpx;
		background: #F5F5F5;
	}

	.top-title {
		padding: 30rpx 4%;
	}

	.top-title .ll {
		width: 80%;
	}

	.top-title .rr {
		width: 15%;
	}

	.fxArrow {
		width: 34rpx;
		height: 12rpx;
		display: block;
		margin: 0 20rpx;
	}

	.switchIcon {
		width: 34rpx;
		height: 37rpx;
		display: block;
	}

	.stationLis {
		margin-top: 40rpx;
	}

	.scroll-item {
		white-space: nowrap;
		position: relative;
	}

	.scroll-item .line {
		padding: 60rpx 0;
	}

	.scroll-item .line.on {
		color: #ff2626;
	}

	.scroll-item .line image {
		width: 100%;
		height: 100%;
		display: block;
	}

	.scroll-item .line .Gps {
		width: 54rpx;
		height: 20rpx;
		position: relative;
	}

	.scroll-item .line .Gps .currIcon {
		width: 30rpx;
		height: 35rpx;
		display: block;
		margin: 0 auto;
		position: absolute;
		left: 50%;
		transform: translate(-50%, -50%);
	}

	.scroll-item .line .xianBJ {
		padding: 4rpx 80rpx 4rpx 30rpx;
		background: #15bcaa;
		margin: 10rpx 0;
	}

	.scroll-item .line .arrow {
		width: 16rpx;
		height: 12rpx;
	}

	.scroll-item .line .infor {
		width: 36rpx;
		margin-left: 20rpx;
	}

	.scroll-item .line .name {
		word-wrap: break-word;
		width: 36rpx;
		margin: 0 auto;
		line-height: 32rpx;
		white-space: initial;
	}

	.scroll-item .line:first-child .xianBJ {
		border-radius: 10rpx 0 0 10rpx;
	}

	.scroll-item .line:last-child .xianBJ {
		border-radius: 0 10rpx 10rpx 0;
		padding-right: 40rpx;
	}
</style>
