<template>
	<view class="worthBottomListItem">
		<view :style="{height:worthBottomListItem.appBanHeight+'rpx'}">
			<imageLazyLoad :src="worthBottomListItem.picUrl"></imageLazyLoad>
		</view>
		<view :class="worthBottomListItem.pubType=== -1?'worthBottomListItemTextActiv':'worthBottomListItemText'">
			<view>{{worthBottomListItem.title}}</view>
			<view></view>
			<view v-if="worthBottomListItem.pubType=== -1">{{worthBottomListItem.subTitle}}</view>
		</view>
		<view v-if="worthBottomListItem.pubType !== -1" class="worthBottomListItemInfo">
			<view>
				<view class="worthBottomListItemInfoLeftImage">
					<imageLazyLoad :src="worthBottomListItem.avatar+'?imageView&quality=65&thumbnail=48y48'" ></imageLazyLoad>
				</view>
				<view class="worthBottomListItemInfoLeftText">{{worthBottomListItem.nickname}}</view>
			</view>
			<view>
				<image class="worthBottomListItemInfoRightImage" src="https://yanxuan.nosdn.127.net/5097bc5f2e1eb15f2a32b56895db073a.png" mode="widthFix"></image>
				<view class="worthBottomListItemInfoRightText">{{worthBottomListItem.readCount}}</view>
			</view>
		</view>
		<view v-if="worthBottomListItem.buyNow" class="worthBuyNow">
			<view>{{worthBottomListItem.buyNow.itemName}}</view>
			<view>
				<text>去购买</text>
				<text class="iconfont icon-iconfontxueqi-copy"></text>
			</view>
		</view>
	</view>
</template>

<script>
	import imageLazyLoad from "../../../components/imageLazyLoad.vue"
	export default {
		props:{
			worthBottomListItem:{
				type:Object,
				default(){
					return {}
				}
			}
		},
		components:{
			imageLazyLoad
		}
	}
</script>

<style lang="stylus" scoped>
	.worthBottomListItem
		width 345rpx
		background-color #FFFFFF
		border-radius 14rpx
		margin-bottom 10px
		overflow hidden
	.worthBottomListItemText
		&>view
			&:nth-child(1)
				padding 18rpx 8rpx 0
				font-size 28rpx
				color #333333
				overflow hidden
				text-overflow ellipsis
				display -webkit-box
				-webkit-box-orient vertical
				-webkit-line-clamp 2
			&:nth-child(2)
				height 18rpx
				background-color #FFFFFF
	.worthBottomListItemTextActiv
		background-color #ffe1b2
		padding 28rpx 18rpx 36rpx
		&>view
			color #333333
			text-align center
			&:nth-child(1)
				font-size 32rpx
			&:nth-child(2)
				width 48rpx
				height 4rpx
				background-color #333333
				margin 21rpx auto 24rpx
			&:nth-child(3)
				font-size 24rpx
				margin-top 18rpx
	.worthBottomListItemInfo
		padding 16rpx 16rpx 24rpx
		display flex
		justify-content space-between
		&>view
			display flex
			align-items center
			.worthBottomListItemInfoLeftImage
				width 48rpx
				height 48rpx
				border-radius 50%
				overflow hidden
			.worthBottomListItemInfoLeftText
				margin-left 8rpx
				font-size 24rpx
				color #7f7f7f
				overflow hidden
				text-overflow ellipsis
				display -webkit-box
				-webkit-box-orient vertical
				-webkit-line-clamp 1
			.worthBottomListItemInfoRightImage
				width 32rpx
				display block
			.worthBottomListItemInfoRightText
				margin-left 2rpx
				font-size 24rpx
				color #7f7f7f
	.worthBuyNow
		padding 20rpx
		display flex
		justify-content space-between
		border-top 1rpx solid #d9d9d9
		overflow hidden
		view
			font-size 24rpx
			&:first-child
				flex 1
				color #333333
				overflow hidden
				text-overflow ellipsis
				display -webkit-box
				-webkit-box-orient vertical
				-webkit-line-clamp 1
			&:last-child
				flex-shrink 0
				color #dd1a21
				display flex
				align-items center
				text
					flex-shrink 0
					&:last-child
						font-size 22rpx
</style>
