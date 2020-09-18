<template>
	<view class="my">
		<view class="header">
			<view class="texts">
				<image src="/static/weizhi.png" mode=""></image>
				<view class="text_v" >
					福州
				</view>
			</view>

			<view class="btns">
				<image src="/static/xinxi.png" mode=""></image>
				<image src="/static/append.png" mode=""></image>
			</view>
		</view>
	<view class="user">
			<image src="/static/icon.png" mode=""></image>
			<view class="data">
				<view class="data_1">
					<view class="name" v-text="nick">
					</view>
					<view class="title" v-text="title">
					</view>
				</view>
				<view class="data_2">
					<view class="phone" v-text="phone">
					</view>
					<view class="pp gray">
						<view class="">
							个人资料
						</view>
						<image src="/static/right.png" mode="" class="right"></image>
					</view>
				</view>
				<view class="data_3 gray" v-text="time">
				</view>
			</view>
	</view>
	<view class="banner3">
		<image src="/static/banner3.png" mode=""></image>
	</view>
	</view>
</template>

<script>
	export default{
		methods:{

		},
		data(){
			return {
				nick:'',
				title:'',
				phone:'',
				time:''
			}
		},
		created:function(){
			this.$req
			.get(this.$api.user_detail)
			.then(r=>{
				if(r.status == 200){
					let data = r.data;
					this.nick ='*' + data.username.slice(1);
					this.title = data.star_level;
					 let str = data.mobile;
					  let pre =  str.substr(0,3);
					  let next = str.substr(7,4);
					  let result  =  pre + '****' + next;
					this.phone = result
					this.time ='上次登录：'+ data.create_time;
				}
			})
		}
	}
</script>

<style lang="less">
	.header{
		width: 100%;
		height: 90.27rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		.texts{
			margin-left: 34.72rpx;
			font-size: 34.72rpx;
			display: flex;
			align-items: center;
			.text_v{
				margin-left: 9rpx;
			}
			image{
				width: 50rpx;
				height: 50rpx;
				}
			}
		.btns{
			image{
				margin-right: 24.3rpx;
				width: 60rpx;
				height:60rpx;
				}
			}
	}
	.user{
		width: 100%;
		display: flex;
		align-items: center;
		padding: 36rpx 10rpx 0 36rpx;
		image{
			width: 150rpx;
			height: 150rpx;
			margin-right: 9rpx;

		}
		.data{
			width: 75%;
			.data_1,.data_2,.data_3{
				margin-bottom: 10rpx;
			}
			.data_1,.data_2{
				display: flex;
				font-size: 31.53rpx;
				align-items: center;
				.title{
					padding: 0 18rpx;
					margin-left: 18rpx;
					font-size: 18rpx;
					background-color: #f3a86a;
					color: #FFFFFF;
					height: 36rpx;
					line-height: 36rpx;
					border-radius: 18rpx;
				}
			}
			.data_2{
				width: 100%;
				justify-content: space-between;
				.pp{
					display: flex;
					align-items: center;
					margin-right: -30rpx;
				}
				.right{
					width: 40rpx;
					height: 40rpx;
				}
			}
		}
		.gray{
			color: #AAAAAA;
		}
		.data_3{
			font-size: 22.52rpx;
		}
	}
		.banner3{
			margin-bottom: 70px;
			image{
				width: 100%;
				height: 1050rpx;
			}
		}
</style>
