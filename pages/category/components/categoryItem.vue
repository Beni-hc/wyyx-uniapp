<template>
	<view class="categoryItemBox">
		<view class="categoryItemBanner">
			<banner v-if="bannerList.length > 1" :bannerList="bannerList"  ></banner>
			<imageLazyLoad v-else :src="bannerList[0].picUrl" ></imageLazyLoad>
		</view>
		<view>
			<view class="categoryItemList" 
			v-for="(categoryGroupListItem,categoryGroupListIndex) in categoryGroupList"
			:key="categoryGroupListIndex"
			>
				<view class="categoryItemListTitle" v-if="categoryGroupListItem.name">{{categoryGroupListItem.name}}</view>
				<view class="categoryItemListcontent">
					<view 
					v-for="categoryListItem in categoryGroupListItem.categoryList"
					:key="categoryListItem.id"
					>
						<imageLazyLoad class="categoryItemListcontentImage" :src="categoryListItem.wapBannerUrl" ></imageLazyLoad>
						<view>{{categoryListItem.name}}</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import banner from "../../../components/banner.vue"
	import imageLazyLoad from "../../../components/imageLazyLoad.vue"
	export default {
		props:["categoryGroupList","bannerList"],
		components:{
			banner,
			imageLazyLoad
		}
	}
</script>

<style lang="stylus" scoped>
	.categoryItemBox
		padding 30rpx
		.categoryItemBanner
			width 100%
			height 192rpx
			margin-bottom 32rpx
		.categoryItemList
			.categoryItemListTitle
				font-size 28rpx
				color #333333
				font-weight 700
				margin-bottom 24rpx
				padding-bottom 8rpx
				border-bottom 1px solid #d9d9d9
			.categoryItemListcontent
				display flex
				flex-flow row wrap
				&>view
					margin-right 48rpx
					overflow hidden
					&:nth-child(3n)
						margin-right 0rpx
					.categoryItemListcontentImage
						width 144rpx
						height 144rpx
						display block
					view
						width 144rpx
						height 72rpx
						font-size 24rpx
						color #333333
						line-height 36rpx
						text-align center
						overflow hidden
						text-overflow ellipsis
						display -webkit-box
						-webkit-box-orient vertical
						-webkit-line-clamp 2
</style>
