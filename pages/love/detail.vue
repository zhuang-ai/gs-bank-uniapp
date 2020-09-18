<template>
	<view class="">
		<view class="detail">
			<view class="status_bar">
			    <view class="top_view"></view>  
			</view>  
			<view class="topbar_u  gray">
				<image src="/static/left_1.png" mode="" @click="goback" class="limg"></image>
				<image src="/static/detail.png" mode="" class="cimg"></image>
				<image src="/static/download.png" mode="" class="eimg"></image>
			</view>
			<view class="num" :class="money.substr(0,1) == '-'?'green':'red'" style="margin-top: 100rpx;">
				{{money.substr(0,1) == '-'?money:'+' + money}}
			</view>
			<image src="/static/newimg1.png" mode="" class="newimg1"></image>
			<view class="data">
				<view class="data_i">
					<view class="data_i_t">
						交易时间
					</view>
					<view class="data_i_v">
						{{time}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						交易卡号
					</view>
					<view class="data_i_v">
						{{cardnum}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						交易账户
					</view>
					<view class="data_i_v">
						{{trading_account}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						交易户名
					</view>
					<view class="data_i_v">
						{{username}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						业务摘要
					</view>
					<view class="data_i_v text-blue">
						{{mclass}} >
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						交易国家或地区简称
					</view>
					<view class="data_i_v">
						{{country}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						交易场所
					</view>
					<view class="data_i_v">
						{{local}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						交易金额
					</view>
					<view class="data_i_v">
						{{money_e}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						记账金额
					</view>
					<view class="data_i_v">
						{{money_e}}
					</view>
				</view>
				<view class="data_i">
					<view class="data_i_t">
						记账币种
					</view>
					<view class="data_i_v">
						{{money_c}}
					</view>
				</view>
				<view class="data_i" style="border-bottom: 1px solid #dbdede;">
					<view class="data_i_t">
						交易卡余额
					</view>
					<view class="data_i_v">
						{{balance}}
					</view>
				</view>
		<view class="data_i" v-if="ocardnum!=''">
			<view class="data_i_t">
				对方账户
			</view>
			<view class="data_i_v">
				{{ocardnum}}
			</view>
		</view>
		<view class="data_i" v-if="oname!=''">
			<view class="data_i_t">
				对方户名
			</view>
			<view class="data_i_v">
				{{oname}}
			</view>
		</view>
		<view class="data_i" v-if="merchant_category!=''">
			<view class="data_i_t">
				对方账户行别
			</view>
			<view class="data_i_v">
				{{merchant_category}}
			</view>
		</view>
				</view>
		</view>
	<image src="/static/newimg2.png" mode="" class="newimg2" v-if="money.substr(0,1) != '-'"></image>
	<request-loading></request-loading>
	</view>
	
</template>

<script>
	export default{
		onLoad(e){
			let id = e.id
			this.$req
			.get(this.$api.account_info + id)
			.then(r=>{
				if(r.status == 200){
					let data = r.data
					console.log(data)
					let money = this.$toThousands(data.tran_amount) 
					let money2 = this.$toThousands(Math.abs(data.tran_amount).toFixed(2)) 
					this.money = money
					this.money_e = money2
					this.mclass = data.pay_info
					this.country = data.short
					this.local = data.trade_floor
					this.balance =  this.$toThousands(data.balance) 
					this.time = data.create_time
					if(data.merchant!=''){
						this.ocardnum = this.$encrypt(data.merchant)
					}
					this.oname = data.merchant_name
					this.money_c = this.list4[data.currency_account].name
					this.username = data.username
					this.trading_account = this.$encrypt(data.trading_account)
					this.merchant_category = data.merchant_category
				};
				this.$req
				.get(this.$api.user_detail)
				.then(r=>{
					if(r.status == 200){
						let data = r.data;
						console.log(data)
						this.cardnum = this.$encrypt(data.card_number)
					}
				})
			})
		},
		data(){
			return {
				list1:[
					{},
					{name:'消费',checked:false,code:1},
					{name:'贷款',checked:false,code:3},
					{name:'工资',checked:false,code:4},
					{name:'支付宝',checked:false,code:5},
					{name:'财付通',checked:false,code:6},	
				],
				list4:[
					{},
					{name:'人民币',checked:false,code:0},
					{name:'美元',checked:false,code:1},
					{name:'港币',checked:false,code:2},
					{name:'欧元',checked:false,code:3},
					{name:'英镑',checked:false,code:4},
					{name:'瑞士法郎',checked:false,code:5},
					{name:'新加坡元',checked:false,code:6},
					{name:'日元',checked:false,code:7},
					{name:'加拿大元',checked:false,code:8},
					{name:'澳大利亚元',checked:false,code:9}
				],
				money_e:'',
				money:'',
				time:'',
				cardnum:'',
				mclass:'',
				country:'',
				local:'',
				money_c:'',
				balance:'',
				ocardnum:'',			
				oname:'',
				username:'',
				merchant_category:'',
				trading_account:''
				}
		},
		methods:{
			goback(){
				this.$showLoading()
				setTimeout(()=>{
					this.$hideLoading()
					uni.navigateBack()
				},500)
				
			}
			
		}
	}
</script>

<style lang="less">
	page{
		background: #efefef;
	}
	.detail{
		background: #FFFFFF;
		margin-bottom: 13.88rpx;
	}
	.newimg1{
		width: 100%;
		height: 79.86rpx;
		margin-top: -17rpx;
	}
	.newimg2{
		width: 100%;
		height: 219.44rpx;
	}
	.topbar_u{
		.cimg{
			height: 41.44rpx;
			width: 144.14rpx;
			margin-right: -50rpx;
		}
		.eimg{
			width: 50.45rpx;
			height: 39.63rpx;
		}
	}
	.num{
		height: 162.16rpx;
		width: 100%;
		text-align: center;
		line-height: 162.16rpx;
		font-size: 50rpx;
	}
	.data{
		padding: 0 27.02rpx 0 27.02rpx;
		margin-bottom: 13.88rpx;
		width: 100%;
		.data_i{
			width: 100%;
			display: flex;
			justify-content: space-between;
			height: 63.06rpx;
			line-height: 63.06rpx;
			font-size: 28rpx;
		}
	}
</style>
