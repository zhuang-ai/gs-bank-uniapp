<template>
	<view class="suixin">
		<view class="status_bar">
		    <view class="top_view"></view>  
		</view>  
		<view class="topbar_u  gray">
			<image src="/static/left_1.png" mode="" @click="goto('/pages/love/query')" class="limg"></image>
			<image src="/static/suixin.png" mode="" class="cimg"></image>
			<image src="/static/icon2.png" mode="" class="eimg"></image>
		</view>
		<image src="/static/banner6.png" mode="" class="img4" v-show="!swshown" style="margin-top: 100rpx;"></image>
		<view class="item1">
			<view class="ii" >
				<view class="iv">
					全部
				</view>
				<image src="/static/arrow_close.png" mode=""></image>
			</view>
			<view class="ii" >
				<view class="iv">
					全部卡
				</view>
				<image src="/static/arrow_close.png" mode=""></image>
			</view>
			<view class="ii" >
				<view class="iv"  @click="modelshow">
					筛选
				</view>
				<image src="/static/shaixuan.png" mode="" class="img2"></image>
			</view>
			<view class="ii" >
				<view class="iv">
					搜索
				</view>
				<image src="/static/icon_search.png" mode="" class="img2"></image>
			</view>
			
		</view>
		<view class="swindow" v-show="swshown">
			<view class="sbox">
			<view class="sitem  pdbn">
				<view class="stitle">
					常用
				</view>
				<view class="sitem_ul sps">
					<view class="sitem_li" v-for="(item,index) in list1" @click="setchoose(list1,index)" :key="index" :class="item.checked?'bg-myred':''">
						{{item.name}}
					</view>
				</view>
			</view>
			<view class="sitem">
				<view class="stitle">
					交易金额
				</view>
				<view class="sitem_ul">
					<view class="sitem_li"  v-for="(item,index) in list2" @click="setchoose(list2,index)" :key="index" :class="item.checked?'bg-myred':''">
						{{item.name}}
					</view>
				</view>
				<view class="sitem_ips">
					<view class="inpbox">
						<image src="/static/money.png" mode=""></image>
						<input type="text" value=""  placeholder="最小金额" class="sitem_ip" v-model="min_money"/>
					</view>
					<image src="/static/heng.png" mode=""></image>
					<view class="inpbox">
						<image src="/static/money.png" mode=""></image>
						<input type="text" value=""  placeholder="最大金额" class="sitem_ip" v-model="max_money"/>
					</view>
				</view>
			</view><view class="sitem">
				<view class="stitle">
					交易时间
				</view>
				<view class="sitem_ul siem_day">
					<view class="sitem_li" v-for="(item,index) in list3" @click="setchoose(list3,index)" :key="index" :class="item.checked?'bg-myred':''">
						{{item.name}}
					</view>
				</view>
				<view class="sitem_ips">
					<view class="inpbox2">
						<image src="/static/rili2.png" mode=""></image>
						<input type="text" value=""  placeholder="开始时间" class="sitem_ip" v-model="start_time"/>
					</view>
					<image src="/static/heng.png" mode=""></image>
					<view class="inpbox2">
						<image src="/static/rili2.png" mode=""></image>
						<input type="text" value=""  placeholder="终止时间" class="sitem_ip" v-model="end_time"/>
					</view>
				</view>
			</view>
			<view class="sitem pdbn">
				<view class="stitle">
					交易币种
				</view>
				<view class="sitem_ul ">
					<view class="sitem_li_2" v-for="(item,index) in list4" @click="setchoose(list4,index)" :key="index" :class="item.checked?'bg-myred':''">
						{{item.name}}
					</view>
				</view>
			</view>
			<view class="mybtns">
				<button type="default" class="btn" @click="reset">重置</button>
				<button type="default" class="btn" @click="searth">确定</button>
			</view>
			</view>
		</view>
		<view class="" v-show="showSearch == 0" v-for="(item,index) in list" :key='index' >
			<view class="banner1" >
				<view class="year">
					{{item.year}}
				</view>
				<view class="box1">
					<view class="text">
						不含当日明细，收支统计仅针对人民币
					</view>
					<image src="/static/rili.png" mode=""></image>
				</view>
				
			</view>
			<view class="data"  v-for="(item2,index2) in item.data" :key="index2">
				<view class="data_t"  @click="setcontain(`${index}${index2}`)">
					<view class="t1">
						<image :src="item2.show?'/static/arrow_open.png':'/static/arrow_close.png'" mode=""></image>
						<view class="t1m" >
							{{item2.month}}月
						</view>
					</view>
					<view class="red">
						+{{item2.pay_in | formatDate}}
					</view>
					<view class="green" v-if="item2.pay_out != 0">
						{{item2.pay_out | formatDate}}
					</view>
				</view>
				<view class="qvalue" @click="goto('/pages/love/detail?id='+item3.id)"  v-for="(item3,index3) in item2.record" :key="index3" v-show="item2.show">
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
							<view class="datas">
								<view class="datac">
									{{item3.pay_info}}
								</view>
								<view class="dataf mn">
									(工银借记卡{{cardnum2}})
								</view>
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
		</view>
		
		<view class="item2" v-if="showSearch == 2">
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
			<view class="query_v" v-for="(item,index) in list" :key="'second' + index" v-if="showSearch == 2">
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
			</view>
			<request-loading></request-loading>
		
	
	</view>
</template>

<script>
	export default{
		data(){
			return {
				weeklist:["日","一","二","三","四","五","六"],
				showSearch:0,
				"shownum":0,
				"swshown":false,
				showList:[],
				min_money:'',
				max_money:'',
				start_time:'',
				end_time:'',
				cardnum2:'',
				list:[],
				list1:[
					{name:'消费',checked:false,code:2},
					{name:'贷款',checked:false,code:4},
					{name:'工资',checked:false,code:5},
					{name:'支付宝',checked:false,code:6},
					{name:'财付通',checked:false,code:7},	
				],
				list2:[
					{name:'小于1千',checked:false},
					{name:'1千-5千',checked:false},
					{name:'5千-1万',checked:false},
					{name:'1万-5万',checked:false},
					{name:'大于5万',checked:false},
				],
				list3:[
					{name:'近一月',checked:false},
					{name:'近三月',checked:false},
					{name:'近半年',checked:false},
					{name:'近一年',checked:false}
				],
				list4:[
					{name:'人民币',checked:false,code:1},
					{name:'美元',checked:false,code:2},
					{name:'港币',checked:false,code:3},
					{name:'欧元',checked:false,code:4},
					{name:'英镑',checked:false,code:5},
					{name:'瑞士法郎',checked:false,code:6},
					{name:'新加坡元',checked:false,code:7},
					{name:'日元',checked:false,code:8},
					{name:'加拿大元',checked:false,code:9},
					{name:'澳大利亚元',checked:false,code:10}
				],
				obj1:[],
				obj2:[]
			}
		},
		methods:{
			reset(){
				let resetfor = function(arr){
					arr.forEach(v=>{
						v.checked = false
					})
				}
				resetfor(this.list1);
				resetfor(this.list2);
				resetfor(this.list3);
				resetfor(this.list4);
				this.obj1 = [];
				this.obj2 = [];
				this.min_money = ''
				this.max_money = ''
				this.start_time = ''
				this.end_time = ''
			},
			searth(){
				this.$showLoading()
				console.log(this.obj1)
				let obj1 = JSON.stringify(this.obj1)
				console.log(obj1)
				this.$req
				.get(this.$api.do_check,{status:obj1,currency_account:this.obj2,start_time:this.start_time,end_time:this.end_time,max_price:this.max_money,min_price:this.min_money})
				.then(r=>{
					if(r.status == 200){
						let data = r.data
						console.log(data)
						if(typeof(data[0].count) != "undefined" && data[0].count == 0){
							this.showSearch = 1
							this.swshown = false
							return
						}
						let output = []
						if(data[0].results){
							data.forEach(v=>{
								output.push(v.results)
							})
						}
						else{
							output = data
						}
						console.log(output)
						let num1 = 0
						let num2 = 0
						output.forEach(v=>{
							v.data.forEach(v2=>{
									num1 += v2.pay_in
									num2 += v2.pay_out
								
							})
						})
							this.add =   num1
						this.remove =  num2
						this.list = output
						setTimeout(()=>{
							this.$hideLoading()
							this.swshown= false
						},500)
						this.showSearch = 2
					}
				})
			},
			setchoose(list,index){
				if(list == this.list1){
					let items = this.list1;
					if(items[index].checked){
						items[index].checked = false
						this.$remove(this.obj1,items[index].code)
						return
					}
					let num = 0
					for(let i=0,l=items.length;i<l;i++ ){
						if(items[i].checked == true){
							num++
						}
					}
					if(num>=3){
						return
					}
					items[index].checked = true
					this.obj1.push(items[index].code)
				}
				else if(list == this.list2){
					let items = this.list2;
					if(items[index].checked){
						items[index].checked = false
						this.min_money = '';
						this.max_money = ''
						return
					}
					items.forEach(v=>{
						v.checked = false
					})
					items[index].checked = true
					switch(index){
						case 0:
						this.min_money = 0;
						this.max_money = 1000;
						break;
						case 1:
						this.min_money = 1000;
						this.max_money = 5000;
						break;
						case 2:
						this.min_money = 5000;
						this.max_money = 10000;
						break;
						case 3:
						this.min_money = 10000;
						this.max_money = 50000;
						break;
						case 4:
						this.min_money = 50000;
						this.max_money = '';
						break;
					}
				}
				else if(list == this.list3){
					let items = this.list3;
					if(items[index].checked){
						items[index].checked = false
						this.end_time = ''
						this.start_time = ''
						return
					}
					items.forEach(v=>{
						v.checked = false
					})
					items[index].checked = true
					let date = new Date();
					    let dateYear = date.getFullYear();             
					    let dateMonth = date.getMonth() + 1;   
							if(dateMonth < 10){
								dateMonth = '0' + dateMonth
							} 
					    let dateDate = date.getDate(); 
						this.end_time = `${dateYear}-${dateMonth}-${dateDate}`;
					switch(index){
						case 0:
						this.start_time = `${dateYear}-${dateMonth - 1}-${dateDate}`;
						break;
						case 1:
						this.start_time = `${dateYear}-${dateMonth - 3}-${dateDate}`;
						break;
						case 2:
						this.start_time = `${dateYear}-${dateMonth - 6}-${dateDate}`;
						break;
						case 3:
						this.start_time = `${dateYear - 1}-${dateMonth}-${dateDate}`;
						break;
					}
				}
				else{
					let items = this.list4;
					if(items[index].checked){
						items[index].checked = false
						this.$remove(this.obj2,items[index].code)
						return
					}
					let num = 0
					for(let i=0,l=items.length;i<l;i++ ){
						if(items[i].checked == true){
							num++
						}
					}
					if(num>=3){
						return
					}
					items[index].checked = true
					this.obj2.push(items[index].code)
				}
			},
			modelshow(){
				this.swshown = !this.swshown
			},
			goto(url){
				this.$showLoading()
				
				setTimeout(()=>{
					this.$hideLoading()
					uni.navigateTo({
						url:url
					})
				},500)
				
			},
			setcontain(e){
				this.list.forEach(v=>{
					v.data.forEach(v2=>{
						if(v2.id == e){
							v2.show = !v2.show
						}
					})
				})
			},
		
		},
		created() {
			this.$req
			.get(this.$api.user_detail)
			.then(r=>{
				if(r.status == 200){
					this.cardnum2 = r.data.card_number.substr(r.data.card_number.length - 4) 
				}
			})
				this.$req
				.get(this.$api.do_check)
				.then(r=>{
					if(r.status == 200){
						let data = r.data
						let output = []
						console.log(data)
						data.forEach((v,i)=>{
							v.data.forEach((v2,i2)=>{
								v2.id = `${i}${i2}`
								if(v2.id =='00'){
									v2.show = true
								}
								else{
									v2.show = false
								}
								
								})
						})
						this.list = data
					}
				})
			},}
</script>

<style lang="less">
	.swindow{
		width: 100%;
		position: fixed;
		top: 172.07rpx;;
		bottom: 0;
		background: rgba(0, 0, 0, 0.6);
		z-index: 999;
		.bg-myred{
			background-color: #e7847a;
			color: #FFFFFF;
		}
		.sbox{
			background: #ebebeb;
		}
		.inpbox,.inpbox2{
			position: relative;
			image{
				width: 32.43rpx;
				height: 36.03rpx;
				position: absolute;
				left: 25.22rpx;
				top: 50%;
				transform: translateY(-50%);
			}
		}
		.inpbox2{
			image{
				width: 45.04rpx;
				height: 41.44rpx;
				left: 261.26rpx;
			}
		}
		.sitem{
			margin-bottom: 18.01rpx;
			background-color: #FFFFFF;
			padding:  18.01rpx;
		}
		.pdbn{
			padding-bottom: 0;
		}
		.mybtns{
			display: flex;
			padding: 25rpx 10rpx 42.34rpx 10rpx;
			.btn{
				width: 354.05rpx;
				background-color: #e7847a;
				font-size: 27rpx;
				color: #FFFFFF;
				height: 76.57rpx;
				text-align: center;
				line-height: 76.57rpx;
			}
		}
		.stitle{
			margin-bottom: 18.01rpx;
		}
		.sitem_ul{
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			.sitem_li,.sitem_li_2{
				margin-bottom: 18.01rpx;
				font-size: 25rpx;
				padding: 18.01rpx;
				border: 1px solid #dbdede;
			}
			.sitem_li_2{
				padding: 0;
				height: 73.87rpx;
				width: 135.13rpx;
				margin-right: 0;
				line-height: 73.87rpx;
				text-align: center;
			}
		}
		.siem_day{
			padding: 0 10rpx;
			.sitem_li{
				padding: 0;
				width: 162.16rpx;
				height: 63.06rpx;
				line-height: 63.06rpx;
				text-align: center;
			}
		}
		.sps{
			justify-content: start;
			.sitem_li{
				margin-right: 18.01rpx;
			}
		}
		.sitem_ips{
			display: flex;
			justify-content: space-between;
			align-items: center;
			image{
				width: 34.23rpx;
				height: 34.23rpx;
			}
			.sitem_ip{
				padding-left: 60rpx;
				width:306.3rpx;
				height: 60.36rpx;
				border: 1px solid #dbdede;;
			}
		}
	}
	.suixin{
		background: #f3f3f3;
	}
	.gray{
		color: #6d747f;
	}
	.topbar_u{

		.cimg{
			height:46.84rpx;
			width: 113.51rpx;
			margin-right: -60rpx;
		}
		.eimg{
			width: 66.66rpx;
			height: 45.04rpx;
		}
	}
	.banner1{
		color: #a9aba0;
		height: 76.57rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 36.03rpx;
		border-bottom: 1px solid #dbdede;
		.box1{
			display: flex;
			align-items: center;
			image{
				width: 54.05rpx;
				height: 49.54rpx;
			}
		}
		
	}
	.img4{
		margin-top: 9rpx;
		width: 100%;
		height: 75.67rpx;
	}
	.img5{
		width: 739.63rpx;
		height: 65.76rpx;
	}
	.item1{
		height: 72.07rpx;
		width: 100%;
		display: flex;
		background-color: #FFFFFF;
		.ii{
			width: 25%;
			display: flex;
			justify-content: center;
			align-items: center;
			border-bottom: 1px #e5e5e5 solid;
			border-right: 1px #e5e5e5 solid;
			image{
				width: 24.32rpx;
				height: 16.21rpx;
				margin-left: 25rpx;
			}
			.img2{
				width: 35rpx;
				height: 35rpx;
			}
		}
	}
	.data_t{
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 81.08rpx;
		background-color: #FFFFFF;
		border-bottom: 1px #e5e5e5 solid;
		padding: 0 27.02rpx;
	
		.t1{
			display: flex;
			align-items: center;
			font-size: 36.03rpx;
			color: #6f7b82;
			image{
				margin-right: 15rpx;
				width: 24.32rpx;
				height: 16.21rpx;
			}
			
		}
	}
	.qvalue{
		background-color: #FFFFFF;
		.qdata{
			.datas{
				display: flex;
				align-items: center;
			}
			.mn{
				margin-left: 9rpx;
			}
			
		}
		
	}
	.item2{
		display: flex;
		align-items: center;
		height: 70rpx;
		justify-content: space-between;
		background-color: #FFFFFF;
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
		padding-left:  22.52rpx;
		border-bottom: 1px #e5e5e5 solid;
		font-size: 36.03rpx;
	}
	.qvalue2{
		padding: 0 27.02rpx;	
	}
</style>
