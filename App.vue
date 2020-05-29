<script>
	import token from "./common/token.js"
	import apis from './apis/index.js'
	export default {
		onLaunch: function() {
			const sapis = apis;
			console.log('App onLaunch')
			plus.push.addEventListener("receive", function(msg) {
				// plus.ui.alert(2);  
				//这里可以写跳转业务代码
				//plus.push.createMessage("您有新的订单！");
				console.log("recevice:" + JSON.stringify(msg));
				/* if (uni.getStorageSync('driverToken') && uni.getStorageSync('isWork') == 1) {
					uni.getLocation({
						type: 'wgs84',
						success: function(res) {
							this.melongitude = res.longitude;
							this.melatitude = res.latitude;
							console.log('this.melatitude',this.melatitude)
							const postData = {};
							postData.tokenFuncName = 'getDriverToken';
							postData.searchItem = {
								no: uni.getStorageSync('driverInfo').info.bus_no,
								user_type: 2
							};
							postData.data = {
								longitude: this.melongitude,
								latitude: this.melatitude
							};
							const callback = (data) => {
								if (data.solely_code == 100000) {
									console.log('hah')
								} else {
									//self.$Utils.showToast(data.msg, 'none', 1000)
								}
							};
							sapis.busUpdate(postData, callback);
						}
					});
				} */
			}, false);
		},
		onShow: function() {
			console.log('App Show..023020')
		},
		onHide: function() {
			console.log('App onHide')
			
		}
	}
</script>

<style>
	@import "/assets/style/style.css";
	@import "/assets/style/quill.css";

	uni-swiper .uni-swiper-dots-horizontal {
		line-height: 16rpx;
		bottom: 5px;
	}

	uni-tabbar .uni-tabbar__icon {
		width: 44rpx;
		height: 44rpx;
	}

	uni-page-head .uni-page-head__title {
		font-weight: normal;
	}

	.ql-editor img {
		max-width: 100%;
		margin: 0 auto;
	}

	uni-page-head .uni-page-head {
		border-bottom: 1px solid #eee;
	}
</style>
