<template>
	<view class="query_contain">
		<view class="status_bar">
		    <view class="top_view"></view>  
		</view>  
		<view class="topbar_u  gray">
			<image src="/static/left_1.png" mode="" @click="goto('/pages/love/user')" class="limg"></image>
			<image src="/static/query.png" mode="" class="cimg"></image>
			<view class="text2">
				更多查询
			</view>
		</view>
		<view class="item1 fiexd">
			<image src="/static/card.png" mode="" class="img1"></image>
			<view class="data1">
				<view class="carddate">
					<view class="cardclass" v-text="cardnum1">
					</view>
					<view class="lastnum" v-text="cardnum2">
					</view>
				</view>
				<view class="cardnum gray" v-text="moneynum">
				</view>
			</view>
			<view class="suixin" @click="goto('/pages/love/suixin')">
				随心查
			</view>
		</view>
	<view class="item2 fiexd">
		<view class="text">
			当页人民币汇总
		</view>
		<view class="red add">
			收：+{{add | formatDate}}
		</view>
		<view class="green remove">
			支：{{remove | formatDate}}
		</view>
	</view>
		<view class="query_v" v-for="(item,index) in list" :key="index" style="margin-top: 290rpx;">
			<view class="" v-for="(item2,index2) in item.data">
				<view class="qbanner">
					{{item.year + '年' + item2.month + '月'}}
				</view>
				<view class="qvalue qvalue2" @click="goto('/pages/love/detail?id='+item3.id)" v-for="(item3,index3) in item2.record" :key="index3">
					<view class="" style="display: flex;">
						<view class="day gray">
							<view class="daynum" :class="new Date(item3.create_time.replace(/-/g, '/')).getDay() == 0||new Date(item3.create_time.replace(/-/g, '/')).getDay() == 6?'myblue':''">
								{{item3.create_time.substr(8,2)}}
							</view>
							<view class="dayweek" :class="new Date(item3.create_time.replace(/-/g, '/')).getDay() == 0||new Date(item3.create_time.replace(/-/g, '/')).getDay() == 6?'myblue':''">
								周{{weeklist[new Date(item3.create_time.replace(/-/g, '/')).getDay()]}}
							</view>
						</view>
						<view class="qdata">
							<view class="datac">
								{{item3.pay_info}}
							</view>
							<view class="dataf">
								{{item3.trade_floor}}
							</view>
						</view>
					</view>
					
					
					<view class="mnum" :class="item3.tran_amount.substr(0,1) == '-'?'green':'red'">
						<text>{{item3.tran_amount.substr(0,1) == '-'?'':'+'}}</text>
						<text>{{item3.tran_amount | formatDate}}</text>
					</view>
				</view>
			</view>
			<request-loading></request-loading>
		</view>


	
	</view>
</template>

<script>
	export default{
		data(){
			return{
				cardnum1:'',
				cardnum2:'',
				add:'',
				remove:'',
				moneynum:'',
				list:{},
				weeklist:["日","一","二","三","四","五","六"]
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
		},
		created() {
			this.$req
			.get(this.$api.do_check)
			.then(r=>{
				if(r.status == 200){
					let data = r.data
					if(data.length == 0){
						return
					}
					let num1 = 0
					let num2 = 0
					console.log(data)
					data.forEach(v=>{
						v.data.forEach(v2=>{
								num1 += v2.pay_in
								num2 += v2.pay_out
							
						})
					})
						this.add =   num1
					this.remove =  num2
					this.list = data
				}
			}),
			this.$req
			.get(this.$api.user_detail)
			.then(r=>{
				if(r.status == 200){
					let data = r.data
					this.cardnum1 = data.card_type
					this.cardnum2 = '尾号' + data.card_number.substr(data.card_number.length - 4) 
					this.moneynum ='人民币余额：' + this.$toThousands(data.balance)
				}
			})
			
		}
	}
</script>

<style lang="less">
	
	.cardclass{
		font-size: 27.77rpx;
	}
	.cimg{
			margin-right: -70rpx;
		}
	.item1{
		top:100rpx;
		left: 0;
		font-size: 22.52rpx;
		height: 120rpx;
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: 1px #e5e5e5 solid;
		padding: 27.02rpx 54.05rpx 31.53rpx 18.01rpx;
		background: #FFFFFF;
		.data1{
			margin-left: -40rpx;
		}
		.carddate{
			display: flex;
			margin-bottom: 18.01rpx;
			align-items: center;
			.lastnum{
				margin-left: 20rpx;
				color: #646464;
			}
		}
		.img1{
			height: 63.06rpx;
			width: 90.09rpx;
		}
		.suixin{
			width: 120rpx;
			text-align: center;
			height: 60rpx;
			line-height: 60rpx;
			border-radius: 60rpx;
			color: #ca9394;
			border: #ca9394 1.5px solid;
		}
	}

	.item2{
		display: flex;
		align-items: center;
		height: 70rpx;
		justify-content: space-between;
		top:220rpx;
		left: 0;
		border-bottom: 1px #e5e5e5 solid;
		background: #FFFFFF;
		width: 100%;
		.text{
			margin-left: 22.52rpx;
		}
		.remove{
			margin-right: 20rpx;
		}

	}
	.qbanner{
		letter-spacing:2rpx;
		background-color: #f5f5f5;
		height: 60rpx;
		line-height: 60rpx;
		padding-left:  22.52rpx;;
		border-bottom: 1px #e5e5e5 solid;
		font-size: 36.03rpx;
	}
	.qvalue2{
		padding: 0 27.02rpx;	
	}

</style>
