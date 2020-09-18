<template>
	<view class="user_contain">
		<view class="status_bar">
		    <view class="top_view"></view>  
		</view>  
		<view class="topbar_u  gray">
			<image src="/static/left_1.png" mode="" @click="goto('/pages/index/index')" class="limg"></image>
			<image src="/static/my.png" mode="" class="cimg"></image>
			<view class="text2">
				更多
			</view>
		</view>
	<view class="box" style="margin-top: 100rpx;">
		<view class="item1">
			<view class="flex">
				<image src="/static/card.png" mode="" class="img1"></image>
				<view class="data1">
					<view class="cardclass">
							{{cardc}}
					</view>
					<view class="cardnum gray" style="display: flex;align-items: center;margin-top: 6.94rpx;">
						<view class=" ">
							{{cardnum}}
					</view>
					<view class="" style="margin-left:20rpx;">
						{{register_type}}
					</view>
					</view>
				</view>
			</view>
			
			<image src="/static/star.png" mode="" class="img2"></image>
		</view>
		<view class="item2">
			<view class="cardclass">
				人民币余额
			</view>
			<view class="cardnum gray">
				<view v-text="money">
				</view>
				<image src="/static/triangle.png" mode="" class="img3"></image>
			</view>
		</view>
		<view class="item3">
			<view class="gray ii" @click="goto('/pages/love/query')">
				查询明细
			</view>
			<view class="gray ii">
				转账汇款
			</view>
			<view class="gray ii">
				开户网点
			</view>
			<view class="gray ii">
				无卡取现
			</view>
			<view class="iis">
				<image src="/static/dian.png" mode=""></image>
			</view>
		</view>
	</view>
	<view class="banner4">
		<image src="/static/banner4.png" mode=""></image>
	</view>
	<request-loading></request-loading>
	</view>
</template>

<script>
	export default{
		created:function(){
			this.$req
			.get(this.$api.user_detail)
			.then(r=>{
				if(r.status == 200){
					let data = r.data;
					console.log(data)
					data.card_type?this.cardc = data.card_type:this.cardc = '福州 借记卡(I)类FzCard'
					this.cardnum = data.card_number;
					this.money = data.balance
					this.register_type = data.register_type
				}
			})
		},
		data(){
			return {
				cardc:'',
				cardnum:'',
				money:'',
				register_type:''
			}
		},
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
		}
	}
</script>

<style lang="less">
	.user_contain{
		background: #f3f3f3;
	}
	.banner4{
		margin-top: 15rpx;
		image{
			width: 100%;
			height: 1017.11rpx;
		}
	}
	.flex{
		align-items: center;
	}
	.box{
		background-color: #FFFFFF;
		.item1,.item2,.item3{
			padding:18.01rpx 25rpx 22.52rpx 20rpx;
			height: 100rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			border-bottom: 1px #e5e5e5 solid;
			.img1{
				height: 63.06rpx;
				width: 90.09rpx;
				margin-right: 50rpx;
			}
			.img2{
				width: 38.73rpx;
				height: 38.73rpx;
			}
			.data1{
				font-size: 31.53rpx;
				.cardnum{
					font-size: 27.02rpx;
				}
			}
		}
		.item2,.item3{
			height:80rpx ;
		}
		.item2{
			padding:18.01rpx 15rpx 22.52rpx 15rpx;
			.cardnum{
				display: flex;
				align-items: center;
				image{
					width: 36.03rpx;
					height: 36.03rpx;
				}
			}
			
		}
		.item3{
			border: none;
			padding: 0;
			justify-content: start;
			.ii,.iis{
				width: calc((100% - 60rpx) / 4);
				text-align: center;
				line-height: 80rpx;
				height: 100%;
				margin: 0;
				border-right: 1px #e5e5e5 solid;
				border-bottom: 1px #e5e5e5 solid;
			}
			.iis{
				width:60rpx;
				image{
					width: 31.53rpx;
					height: 20.72rpx;
				}
			}
		}
	}
</style>
