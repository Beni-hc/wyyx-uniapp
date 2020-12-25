<template>
	<view class="homeBody" >
		<view v-if="bannerList.length > 0" class="homeBodyBannerBox">
			<banner :bannerList="bannerList" ></banner>
		</view>
		<!-- 网易自营品牌logo及分类列表 -->
		<view class="classify" v-if="policyDescList.length > 0 || kingKongList.length > 0">
		    <view class="promise" v-if="policyDescList.length > 0" >
		        <view
		            v-for="(policyDescItem, policyDescIndex) in policyDescList"
		            :key="policyDescIndex"
		        >
		            <image class="promiseImage" :src="policyDescItem.icon" mode="aspectFit" />
					<text>{{policyDescItem.desc}}</text>
		        </view>
		    </view>
		    <view class="classList" v-if="kingKongList.length > 0" >
		        <view
		            v-for="(kingKongItem, kingKongIndex) in kingKongList"
		            :key="kingKongIndex"
		        >
		            <image class="classListImage" :src="kingKongItem.picUrl" mode="aspectFit" />
		            <text>{{ kingKongItem.text }}</text>
		        </view>
		    </view>
		</view>
		<!-- 图片广告展示区域 -->
		<view class="floorListCss" v-if="floorList.length > 0">
			<view :class="{floorListItemActivCss:floorItem.cells.length > 1}" v-for="floorItem in floorList" :key='floorItem.taskId'>
				<view v-for="cellsItem in floorItem.cells" :key='cellsItem.id'>
					<imageLazyLoad :src="cellsItem.picUrl" ></imageLazyLoad>
				</view>
			</view>
		</view>
		<!-- 新人专享礼 -->
		<view class="indexActivityModuleBox" v-if="indexActivityModule.length > 0" >
			<view class="indexActivityModuleTitle">
				<text>新人专享礼</text>
			</view>
			<view class="indexActivityModuleContent">
				<view class="indexActivityModuleContentLeft">
					<view>新人专享礼包</view>
					<view>
						<imageLazyLoad :src="'https://yanxuan.nosdn.127.net/352b0ea9b2d058094956efde167ef852.png'" :mode="aspectFit" ></imageLazyLoad>
						<view></view>
					</view>
				</view>
				<view class="indexActivityModuleContentRight" v-for="(indexActivityItem,indexActivityIndex) in indexActivityModule" :key="indexActivityIndex">
					<view class="indexActivityModuleContentRightTitle">{{indexActivityItem.title}}</view>
					<view 
					:class="indexActivityItem.subTitle ? 'indexActivityModuleContentRightText':'indexActivityModuleContentRightTextTag'" 
					>{{indexActivityItem.subTitle || indexActivityItem.tag}}</view>
					<view class="indexActivityModuleContentRightImage">
						<imageLazyLoad :src="indexActivityItem.showPicUrl" ></imageLazyLoad>
						<view>
							<view>{{indexActivityItem.activityPrice}}</view>
							<view>{{indexActivityItem.originPrice}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 类目热销榜 -->
		<view class="categoryHotSellModuleBox" v-if="categoryHotSellModule.categoryList" >
			<view class="categoryHotSellModuleTitle">{{categoryHotSellModule.title}}</view>
			<view class="categoryHotSellModuleContent">
				<view v-for="(categoryListItem,categoryListIndex) in categoryHotSellModule.categoryList" :key="categoryListIndex">
					<view>{{categoryListItem.categoryName}}</view>
					<imageLazyLoad class="categoryHotSellModuleContentImage" :src="categoryListItem.showPicUrl" ></imageLazyLoad>
				</view>
			</view>
		</view>
		<!-- 新品首发 -->
		<view class="newItemListBox" v-if="newItemList.length > 0" >
			<view class="newItemListTitle">
				<view>新品首发</view>
				<view>
					<text>更多</text>
					<text class="iconfont icon-iconfontxueqi-copy iconfontColor"></text>
				</view>
			</view>
			<view class="newItemListContent">
				<view class="newItemListContentItem" v-for="newItemListItem in newItemList" :key="newItemListItem.id">
					<imageLazyLoad class="newItemListContentItemImage" :src="(newItemListItem.newItemFlag ? newItemListItem.listPicUrl:newItemListItem.primaryPicUrl)" ></imageLazyLoad>
					<view class="newItemListContentItemText">{{newItemListItem.name}}</view>
					<view class="newItemListContentItemPrice">¥{{newItemListItem.retailPrice}}</view>
					<view class="newItemListContentItemTag">
						<view v-if="itemTagListItem.tagId" v-for="itemTagListItem in newItemListItem.itemTagList" :key="itemTagListItem.tagId">{{itemTagListItem.name}}</view>
					</view>
				</view>
			</view>
		</view>
		<!-- 超值专区，员工精选 -->
		<view class="sceneLightShoppingGuideModuleBox" v-if="sceneLightShoppingGuideModule.length > 0" >
			<view class="sceneLightShoppingGuideModuleContent" v-for="(sceneLightItem,sceneLightIndex) in sceneLightShoppingGuideModule" :key="sceneLightIndex">
				<view class="sceneLightShoppingGuideModuleTitle" :style="{color:`#${sceneLightItem.styleItem.titleColor}`}">{{sceneLightItem.styleItem.title}}</view>
				<view class="sceneLightShoppingGuideModuleText" :style="{color:`#${sceneLightItem.styleItem.descColor}`}">{{sceneLightItem.styleItem.desc}}</view>
				<imageLazyLoad 
				class="sceneLightShoppingGuideModuleImage"
				:src="picBeanListItem.picUrl" 
				mode="aspectFit" 
				v-for="picBeanListItem in sceneLightItem.styleItem.itemPicBeanList.slice(0,2)" 
				:key="picBeanListItem.itemId"
				></imageLazyLoad>
			</view>
		</view>
		<!-- 底部版权声明 -->
		<view class="tagend" v-if="sceneLightShoppingGuideModule.length > 0" >
			<view class="tagendAPP">
				<view>下载APP</view>
				<view>电脑版</view>
			</view>
			<view class="tagendBanquan">
				<view>网易公司版权所有 © 1997-2020</view>
				<view>食品经营许可证： JY13301080111719</view>
			</view>
		</view>
	</view>
</template>
<script>
	import banner from "../../../components/banner.vue"
	import imageLazyLoad from "../../../components/imageLazyLoad.vue"
	export default {
		props:{
			bannerList:{
				type:Array,
				default(){
					return []
				}
			},
			policyDescList:{
				type:Array,
				default(){
					return []
				}
			},
			kingKongList:{
				type:Array,
				default(){
					return []
				}
			},
			floorList:{
				type:Array,
				default(){
					return []
				}
			},
			indexActivityModule:{
				type:Array,
				default(){
					return []
				}
			},
			categoryHotSellModule:{
				type:Object,
				default(){
					return {}
				}
			},
			newItemList:{
				type:Array,
				default(){
					return []
				}
			},
			sceneLightShoppingGuideModule:{
				type:Array,
				default(){
					return []
				}
			}
		},
		components:{
			banner,
			imageLazyLoad
		}
	}
</script>

<style lang="stylus" scoped>
	.homeBody
		padding 176rpx 0 0 0
		background-color #EEEEEE
	.homeBodyBannerBox
		height 296rpx
// <!-- 网易自营品牌logo及分类列表样式 -->
	.classify
		height 446rpx
		background-color #fff
		.promise
			height 74rpx
			display flex
			padding 0 20rpx
			justify-content space-between
			view
				display flex
				align-items center
				.promiseImage
					width 32rpx
					height 32rpx
					margin-right 8rpx
				text
					font-size 23rpx
					color #333333
		.classList
			height 372rpx
			display flex
			flex-flow wrap
			justify-content space-between
			view
				width 106rpx
				height 146rpx
				margin 16rpx 20rpx
				text-align center
				.classListImage
					width 106rpx
					height 106rpx
				text
					font-size 23rpx
					color #313131
	// 图片广告展示区域Css
	.floorListCss
		background-color #EEEEEE
		overflow hidden
		&>view
			min-height 80rpx
			display flex
			margin-bottom 20rpx
			view
				flex 1
			&.floorListItemActivCss
				view
					&:first-child 
						margin 0 5rpx 0 20rpx
					&:last-child
						margin 0 20rpx 0 5rpx
	// 新人专享礼
	@keyframes youjiantou
		0%
			background-image url(https://yanxuan.nosdn.127.net/3f7e731c6650bc1770589c78bd5a18b7.png)
		50%
			background-image url(https://yanxuan.nosdn.127.net/d635ac1cf1b2bde41e96ea43dacf0cfc.png)
		100%
			background-image url(https://yanxuan.nosdn.127.net/bf881c47a51af89a723d97d58b2bca9e.png)
	.indexActivityModuleBox
		background-color #FFFFFF
		.indexActivityModuleTitle
			height 90rpx
			text-align center
			line-height 90rpx
			text
				height 100%
				font-size 32rpx
				color #333333
				position relative
				&::after
					content ''
					width 24rpx
					height 3rpx
					position absolute
					right -40rpx
					top 0
					bottom 0
					margin auto
					background-color #333333
				&::before
					content ''
					width 24rpx
					height 3rpx
					position absolute
					left -40rpx
					top 0
					bottom 0
					margin auto
					background-color #333333
		.indexActivityModuleContent
			height 434rpx
			padding 0 20rpx 30rpx 20rpx
			display flex
			flex-flow column wrap
			justify-content space-between
			align-content space-between
			&>view
				&:nth-child(2)
					background-color #fbe2d3
				&:nth-child(3)
					background-color #ffecc2
			.indexActivityModuleContentLeft
				width 352rpx
				height 434rpx
				background-color #f9e9cf
				border-radius 4rpx
				&>view
					&:nth-child(1)
						font-size 32rpx
						color #333333
						padding 30rpx 0 0 30rpx
					&:nth-child(2)
						width 260rpx
						height 260rpx
						margin 36rpx auto
						position relative
						view
							width 30rpx
							height 20rpx
							position absolute
							top 222rpx
							right 65rpx
							background-image url(https://yanxuan.nosdn.127.net/3f7e731c6650bc1770589c78bd5a18b7.png)
							background-repeat no-repeat
							background-size 100% 100%
							animation youjiantou 1s linear infinite
			.indexActivityModuleContentRight
				width 322rpx
				height 194rpx
				padding 20rpx 0 0 30rpx
				border-radius 4rpx
				position relative
				.indexActivityModuleContentRightTitle
					font-size 32rpx
					color #333333
					margin-bottom 4rpx
				.indexActivityModuleContentRightText
					font-size 24rpx
					color #7f7f7f
				.indexActivityModuleContentRightTextTag
					display inline-block
					font-size 24rpx
					color #FFFFFF
					background-color #cbb693
					border-radius 4rpx
					padding 0 10rpx
					line-height 32rpx
				.indexActivityModuleContentRightImage
					width 200rpx
					height 200rpx
					position absolute
					bottom 0
					right 0
					&>view	
						width 80rpx
						height 70rpx
						position absolute
						top 0
						right 30rpx
						opacity .8
						background-color #f59524
						border-radius 50%
						padding-top 10rpx
						view
							color #FFFFFF
							text-align 	center
							&:first-child
								font-size 24rpx
							&:last-child
								font-size 20rpx
								margin-top 7rpx
								text-decoration line-through
	// 类目热销榜
	.categoryHotSellModuleBox
		margin-bottom 20rpx
		background-color #FFFFFF
		.categoryHotSellModuleTitle
			height 100rpx
			font-size 32rpx
			color #333333
			line-height 100rpx
			margin 0 20rpx
		.categoryHotSellModuleContent
			padding 0 10rpx 20rpx 20rpx
			display flex
			flex-flow row wrap
			&>view
				margin 0 10rpx 10rpx 0
				color #333333
				&:nth-child(1),&:nth-child(2)
					width 350rpx
					height 200rpx
					position relative
					view
						font-size 28rpx
						margin 66rpx 0 0 24rpx
						position relative
						&::after
							content ''
							width 48rpx
							height 4rpx
							position absolute
							left 0
							bottom -16rpx
							background-color #333333
					.categoryHotSellModuleContentImage
						width 200rpx
						height 200rpx
						display block
						position absolute
						top 0
						right 0
				&:nth-child(1)
					background-color #f9f3e4
				&:nth-child(2)
					background-color #ebeff6
				&:nth-child(n+3)
					width 170rpx
					height 180rpx
					background-color #f5f5f5
					view
						font-size 24rpx
						padding-top 10rpx
						text-align center
					.categoryHotSellModuleContentImage
						width 120rpx
						height 120rpx
						display block
						margin 0 auto
	// 新品首发
	.newItemListBox
		background-color #FFFFFF
		margin-bottom 20rpx
		.newItemListTitle
			height 100rpx
			padding 0 20rpx
			display flex
			justify-content space-between
			align-items center
			&>view
				color #333333
				&:nth-child(1)
					font-size 32rpx
				&:nth-child(2)
					font-size 28rpx
					.iconfontColor
						color #666666
		.newItemListContent
			padding 0 20rpx 20rpx 20rpx
			display flex
			flex-flow row wrap
			justify-content space-between
			.newItemListContentItem
				width 224rpx
				height 430rpx
				.newItemListContentItemImage
					width 224rpx
					height 224rpx
					display block
					background-color #f5f5f5
				.newItemListContentItemText
					font-size 24rpx
					color #333333
				.newItemListContentItemPrice
					font-size 32rpx
					color #dd1a21
					margin 6rpx 0
				.newItemListContentItemTag
					font-size 18rpx
					color #dd1a21
					display flex
					justify-content space-between
					view
						height 24rpx
						padding 0 10rpx
						border-radius 20rpx
						line-height 24rpx
						border 1px solid #dd1a21
	// 超值专区，员工精选
	.sceneLightShoppingGuideModuleBox
		background-color #FFFFFF
		margin-bottom 20rpx
		padding 14rpx 20rpx 20rpx 20rpx
		display flex
		justify-content space-between
		.sceneLightShoppingGuideModuleContent
			padding 30rpx 22rpx 0 22rpx
			background-color #f5f5f5
			.sceneLightShoppingGuideModuleTitle
				font-size 32rpx
				padding-left 10rpx
			.sceneLightShoppingGuideModuleText
				font-size 24rpx
				padding-left 10rpx
			.sceneLightShoppingGuideModuleImage
				width 150rpx
				height 150rpx
				display inline-block
				margin-right 4rpx
	//底部版权声明
	.tagend
		margin-top 20rpx
		padding 56rpx 20rpx 28rpx
		background-color #414141
		.tagendAPP
			margin-bottom 36rpx
			display flex
			justify-content center
			view
				width 172rpx
				height 62rpx
				margin 0 25rpx
				border-radius 2px
				border 1px solid #999999
				font-size 24rpx
				color #FFFFFF
				line-height 62rpx
				text-align center
		.tagendBanquan
			text-align center
			view
				color #999
				font-size 24rpx
</style>
