<template>
	<view class="request-loading-view" v-show="loadingShow">
		<view class="loading-view">
			<view class="loading" style="width: 30rpx">
			</view>
			<view class="" style="color: #FFFFFF;font-size: 30rpx;">
				正在载入...
			</view>
			<view class="cuIcon-close" style="color: #FFFFFF;font-size: 40rpx;">
				
			</view>
			</view>
	</view>
</template>
 
<script>
export default {
	data() {
		return {};
	},
	computed: {
		//计算属性判断vuex中的显示状态
		loadingShow() {
			return this.$store.state.requestLoading;
		}
	}
};
</script>
 
<style scoped>
.request-loading-view {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	z-index: 999;
	background-color: rgba(0, 0, 0, 0.001);
	display: flex;
	justify-content: center;
	align-items: center;
}
.loading-view {
	width: 250upx;
	height: 60upx;
	background-color: rgba(0, 0, 0, 0.6);
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 6.94rpx;
}
 
/* 动画样式 */
 
</style>