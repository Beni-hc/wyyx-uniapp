<template>
	<view v-if="worthBannerList.length" class="worthBannerBox">
		<swiper class="worthBannerSwiper" display-multiple-items="4" :duration="500" @change="dotIndex">
			<swiper-item class="worthBannerSwiperItem" v-for="(listItem,listIndex) in worthBannerList" :key="listIndex">
				<view class="worthBannerItem" v-for="(item,index) in listItem" :key="index">
					<view class="worthBannerItemImage">
						<imageLazyLoad :src="item.picUrl" ></imageLazyLoad>
					</view>
					<view class="worthBannerItemTitle">{{item.mainTitle}}</view>
					<view class="worthBannerItemText">{{item.viceTitle}}</view>
				</view>
			</swiper-item>
		</swiper>
		<view class="swiperDot">
			<view 
			class="iconfont" 
			:class="dotsIndex===itemIcon?'icon-douhao':'icon-dianxuan'" 
			v-for="(itemIcon) in 5" 
			:key="itemIcon"
			>
			</view>
		</view>
	</view>
</template>

<script>
	import imageLazyLoad from "../../../components/imageLazyLoad.vue"
	export default {
		props:{
			worthBannerList:{
				type:Array,
				default:function(){
					return []
				}
			}
		},
		data(){
			return{
				dotsIndex: 0
			}
		},
		methods:{
			dotIndex(e){
				this.dotsIndex = e.detail.current	
			}
		},
		components:{
			imageLazyLoad
		}
	}
</script>

<style lang="stylus" scoped>
	.worthBannerBox
		height 540rpx
		padding 36rpx 0 70rpx 0
		margin 0 20rpx
		box-sizing border-box
		position relative
		background-color #FFFFFF
		border-radius 12rpx
		.worthBannerSwiper
			height 100%
			.worthBannerSwiperItem
				display flex
				flex-flow column nowrap
				justify-content space-between
				.worthBannerItem
					width 180rpx
					text-align center
					.worthBannerItemImage
						width 120rpx
						height 120rpx
						margin 0 auto
					.worthBannerItemTitle
						font-size 28rpx
						color #333333
						font-weight 700
						margin-top 8rpx
					.worthBannerItemText
						font-size 24rpx
						color #999999
						margin-top 2rpx
	.swiperDot
		position absolute
		left 0
		bottom 20rpx
		right 0
		display flex
		justify-content center
	.icon-douhao
		color #d81e06
		font-size 22rpx
		margin 0 3rpx
	.icon-dianxuan
		color #d9d9d9
		font-size 16rpx
		margin 0 6rpx
</style>
