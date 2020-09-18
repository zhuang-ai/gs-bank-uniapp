<template>
	<view>
		<view class="status_bar">  
		    <view class="top_view"></view>  
		</view>  
		<love v-if="PageCur=='love'"></love>
		<my v-if="PageCur=='my'"></my>
		<view class="cu-bar tabbar bg-white shadow foot">
			<view class="action" @click="NavChange" data-cur="love">
				<view class='cuIcon-cu-image'>
					<image :src="'/static/tabbar/love' + [PageCur=='love'?'_cur':''] + '.png'"></image>
				</view>
				<view :class="PageCur=='love'?'red':''">最爱</view>
			</view>
			<view class="action" >
				<view class='cuIcon-cu-image'>
					<image src="/static/tabbar/card.png"></image>
				</view>
				<view >信用卡</view>
			</view>
			<view class="action" >
				<view class='cuIcon-cu-image'>
					<image src="/static/tabbar/fuwu.png"></image>
				</view>
				<view>智服务</view>
			</view>
			<view class="action" >
				<view class='cuIcon-cu-image'>
					<image src="/static/tabbar/jingxuan.png"></image>
				</view>
				<view>惠精选</view>
			</view>
			<view class="action" @click="NavChange" data-cur="my">
				<view class='cuIcon-cu-image'>
					<image :src="'/static/tabbar/my' + [PageCur == 'my'?'_cur':''] + '.png'"></image>
				</view>
				<view :class="PageCur=='my'?'red':''">我的</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
		return {
				PageCur: 'love'
			}
		},
		methods: {
			NavChange: function(e) {
				this.PageCur = e.currentTarget.dataset.cur
			}
		}
	}
</script>

<style>
	.status_bar {  
	    height: var(--status-bar-height);  
	    width: 100%;  
	    background-color: #000000;  
	}  
	.top_view {  
	    height: var(--status-bar-height);  
	    width: 100%;  
	    position: fixed;  
	    background-color: #000000;  
	    top: 0;  
	    z-index: 999;  
	}
	.cu-bar{
		height: 137.83rpx;
	}
	.red{
		color: #ea625b;
	}
</style>
