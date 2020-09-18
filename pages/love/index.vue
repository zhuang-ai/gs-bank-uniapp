<template>
	<view >
		<view class="header">
			<view class="texts">
				退出
			</view>
			<view class="search">
				<image src="/static/icon_search.png" mode="" class="searimg"></image>
				<input type="text" value="" placeholder="搜功能 搜产品" class="searinp" />
				<image src="/static/mic.gif" mode="" class="mic"></image>
			</view>
			<view class="btns">
				<image src="/static/xinxi.png" mode=""></image>
				<image src="/static/append.png" mode=""></image>
			</view>
		</view>
		<swiper class="screen-swiper my-screen" :autoplay="true" interval="5000" duration="500" >
			<swiper-item v-for="(item,index) in swiperList" :key="index" style="height: 260rpx;">
				<image :src="item.url" mode="aspectFill" v-if="item.type=='image'" style="height: 260rpx;"></image>
			</swiper-item>
		</swiper>
		<view class="swiper2">
			<image src="/static/logo.png" mode="" class="logo"></image>
			<swiper class="screen-swiper " :autoplay="true" interval="5000" duration="200" vertical="true" style="width: 615.27rpx;height: 115.27rpx;">
				<swiper-item v-for="(item,index) in swiperList2" :key="index" style="width: 615.27rpx;height: 115.27rpx;">
					<image :src="item.url" mode="aspectFill" v-if="item.type=='image'" style="width: 615.27rpx;height: 115.27rpx;"></image>
				</swiper-item>
			</swiper>
		</view>
		
		<view class="banner2">
			<view class="user" @click="goto('/pages/love/user')">
				
			</view>
			<image src="/static/banner2.png" mode="" class="img1"></image>
		</view>
		<view class="banner3">
			<view class="value3">
				<view class="datenum">
					<view class="num1">
						{{day>=10?day:'0' + day}}
					</view>
					<view class="num2">
						<view class="num2_ym">
							{{year}}年{{month}}月
						</view>
						<view class="num2_week">
							星期{{week}}
						</view>
					</view>
				</view>
				<view class="text">
					点击获取天气信息
				</view>
			</view>
			<view class="value4">
				亲爱的{{nick}}，{{time}}好
			</view>
			<image src="/static/myimg1.png" mode="" class="bg-img1"></image>
			<image src="/static/myimg2.png" mode="" class="bg-img2"></image>
			<image src="/static/sun.gif" mode="" class="sun"></image>
			<image src="/static/banner7.png" mode="" class="bg-img3"></image>
		</view>
		<request-loading></request-loading>
	</view>
</template>

<script>
	export default{
		methods:{
			goto(url){
				this.$showLoading()
				
				setTimeout(()=>{
					this.$hideLoading()
					uni.navigateTo({
						url:url
					})
				},500)
				
			}
		},
		created:function(){
			//#ifdef APP-PLUS
			// 获取设备信息
				plus.device.getInfo({
					success:function(e){
						this.$req
						.post(this.$api.tag_admin,e.uuid)
						.then(r=>{
							console.log(r)
						})
						console.log('success: '+e.uuid);
					},
					fail:function(e){
						console.log('getDeviceInfo failed: '+JSON.stringify(e));
					}
				});
			//#endif
			this.$req
			.get(this.$api.user_detail)
			.then(r=>{
				if(r.status == 200){
					let data = r.data;
					this.nick ='*' + data.username.slice(1);
					let date = new Date();
						this.year = date.getFullYear();             
					    this.month = date.getMonth() + 1;               
					    this.day = date.getDate();    
					    let week = date.getDay();   
										switch(week){
											case 0:
											this.week = "日";
											break;
											case 1:
											this.week = "一";
											break;
											case 2:
											this.week = "二";
											break;
											case 3:
											this.week = "三";
											break;
											case 4:
											this.week = "四";
											break;
											case 5:
											this.week = "五";
											break;
											case 6:
											this.week = "六";
											break;
										}
					    let dateHours = date.getHours();  
						  if(dateHours>=19||dateHours<=5){
							  this.time = '晚上'
						  }
						  else if(dateHours>=6&&dateHours<12){
							  this.time = '上午'
						  }
						  else if(dateHours>=12&&dateHours<14){
							  this.time = '中午'
						  }
						  else{
							  this.time = '下午'
						  }
					
				}
			})
		},
		data(){
			return {
				nick:'',
				time:'',
				day:'',
				year:'',
				month:'',
				week:'',
				swiperList: [{
					id: 0,
					type: 'image',
					url: '/static/ban1.png'
				}, {
					id: 1,
					type: 'image',
					url: '/static/ban2.png'
				}, {
					id: 2,
					type: 'image',
					url: '/static/ban3.png'
				}, {
					id: 3,
					type: 'image',
					url: '/static/ban4.png'
				}, {
					id: 4,
					type: 'image',
					url: '/static/ban5.png'
				}, {
					id: 5,
					type: 'image',
					url: '/static/ban6.png'
				}],
				swiperList2: [{
					id: 0,
					type: 'image',
					url: '/static/text1.png'
				}, {
					id: 1,
					type: 'image',
					url: '/static/text2.png'
				}, {
					id: 2,
					type: 'image',
					url: '/static/text3.png'
				}, {
					id: 3,
					type: 'image',
					url: '/static/text4.png'
				}, {
					id: 4,
					type: 'image',
					url: '/static/text5.png'
				}]
			}
		}
	}
</script>

<style lang="less">
	.my-screen{
		margin: 18.01rpx;
		margin-top:90rpx;
	}
	.swiper2{
		margin-top: -120rpx;
		display: flex;
		justify-content: space-between;
		.logo{
			margin:15rpx 20rpx;
			width: 100.69rpx;
			height: 84.02rpx;
		}
	}
	.value3{
		display: flex;
		width: 100%;
		height: 131.94rpx;
		padding: 50rpx;
		align-items: center;
		justify-content: space-between;
		z-index: 333;
		position: absolute;
		top: 0;
		left: 0;
		.datenum{
			display: flex;
			align-items: center;
			.num1{
				font-size: 55.55rpx;
				margin-right: 30rpx;
				font-weight: 700;
			}
			.num2_week{
				font-size: 28rpx;
				font-weight: 600;
			}
		}
		.text{
			color: #FFFFFF;
			font-size: 25rpx;
			width: 100rpx;
			font-weight: 500;
		}
	}
	.value4{
		color: #FFFFFF;
		position: absolute;
		top: 152.77rpx;
		left: 65rpx;
		z-index: 333;
		font-size: 30rpx;
		font-weight: 600;
	}
	.header{
		width: 100%;
		height: 90rpx;
		display: flex;
		align-items: center;
		position: fixed;
		top: var(--status-bar-height);
		left: 0;
		z-index: 999;
		background-color: #FFFFFF;
		.texts{
			font-size: 34.72rpx;
			padding: 41.66rpx;
		}
		.search{
			position: relative;
			.searimg{
				width: 39.63rpx;
				height:39.63rpx;
				position: absolute;
				left: 20.83rpx;
				bottom: 50%;
				transform: translateY(40%);
			}
			.mic{
				width: 55.55rpx;
				height: 55.55rpx;
				position: absolute;
				right: 6.94rpx;
				bottom: 50%;
				transform: translateY(50%);
			}
			.searinp{
				font-size: 27.77rpx;
				height: 69.44rpx;
				background-color: #f4f4f4;
				border-radius:34.72rpx ;
				line-height: 69.44rpx;
				padding-left: 62.5rpx;
			}
		}
		.btns{
			position: absolute;
			right: 0;
			image{
				margin-right: 24.3rpx;
				width: 60rpx;
				height:60rpx;
			}
		}
	}
		.banner2{
			margin-top:-280rpx;
			position: relative;
			.img1{
				width:100%;
				height: 517.36rpx
			}
			.user{
				width: 25%;
				height: 33%;
				position: absolute;
				top: 25rpx;
				left:0;
				z-index:99 ;
			}
		}
		.banner3{
			position: relative;
			image{
				width: 100%;
			}
			.bg-img2{
				position: absolute;
				right: 0;
				top:0;
				height: 280.55rpx;
				z-index: 222;
			}
			.bg-img3{
				position: absolute;
				right: 0;
				top:260rpx;
				height: 5325rpx;
				margin-bottom: 136rpx;
			}
			.sun{
				width: 250rpx;
				height: 160rpx;
				position: absolute;
				right: 0;
				top: 0;
				z-index: 111;
			}
		}
		.img2{
			width:72.07rpx;
			height: 72.07rpx;
		}

</style>
