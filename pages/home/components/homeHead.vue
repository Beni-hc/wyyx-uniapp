<template>
	<view class="headBox" :style="{'padding-top': menuButtonTop+'px'}">
		<!-- logo和搜索框 -->
		<view class="headTitle" :style="{margin:` 0 ${menuButtonWidth+menuButtonRight}px 0 ${menuButtonRight}px`}">
			<image :style="{width:menuButtonWidth+'px',height:menuButtonHeight+'px'}" src="../../../static/wyyxlogo/wyyxlogo.png" mode="aspectFit"></image>
			<view :style="{margin:` 0 ${menuButtonRight}px`,'line-height':menuButtonHeight+'px'}">
				<text class="iconfont icon-sousuo"></text>
				<text>搜索{{searchData}}款好物</text>
			</view>
		</view>
		<!-- 横向滑动导航栏 -->
		<view class="headNav">
			<!-- 下拉导航 -->
			<view v-show="isActivRotateZ" class="activNav">
				<view>全部频道</view>
				<view>
					<view 
					:class="{'activRed':activItemRed === indexZ}"
					v-for="(activItemZ,indexZ) in listNav" :key="indexZ" 
					@click="clickActivNav(indexZ,false)"
					>{{activItemZ}}</view>
				</view>
			</view>
			<!-- 横向滚动导航 -->
			<view v-show="!isActivRotateZ">
				<scroll-view class="headNavLeft" :scroll-into-view="scrollToId" scroll-x="true" enable-flex="true" scroll-with-animation="true">
					<view class="headNavLeftItem">
						<view 
						:id="'scrollToID-'+index"
						:class="{'activItem':activItemRed === index}" 
						v-for="(activItem,index) in listNav" :key="index" 
						@click="clickActivNav(index,true)"
						>{{activItem}}</view>
					</view>
				</scroll-view>
			</view>
			<!-- 右侧旋转按钮 -->
			<view class="headNavRight" @click="activRotateZ">
				<view></view>
				<text :class="{'activRotateZ': isActivRotateZ}" class="iconfont icon-iconfontplatformentrance-copy"></text>
			</view>
		</view>
	</view> 
</template>

<script>
	export default {
		props:["searchData"],
		data(){
			return {
				menuButtonTop:0,
				menuButtonRight:0,
				menuButtonWidth:0,
				menuButtonHeight:0,
				isActivRotateZ:false,
				activItemRed: 0,
				scrollToId:null,
				listNav:[
                    "推荐",
                    "居家生活",
                    "服饰鞋包",
                    "美食酒水",
                    "个护清洁",
                    "巨婴亲子",
                    "运动旅行",
                    "数码家电",
                    "严选全球",
                ]
			}
		},
		methods:{
			_getMenuButtonInfo(){
				let a = uni.getMenuButtonBoundingClientRect()
				let	windowWidth = uni.getSystemInfoSync().windowWidth
				this.menuButtonTop = a.top
				this.menuButtonRight = windowWidth - a.right
				this.menuButtonWidth = a.width
				this.menuButtonHeight = a.height
			},
			activRotateZ(){
				this.isActivRotateZ = !this.isActivRotateZ
			},
			clickActivNav(index,isNav){
				if(!isNav){
					this.isActivRotateZ = isNav
				}
				this.activItemRed = index
				this.scrollToId = 'scrollToID-'+index
			}
		},
		created() { 
			this._getMenuButtonInfo()
		}
	}
</script>

<style lang="stylus" scoped>
	.headBox
		background-color #FFFFFF
		position fixed !important
		top 0
		right 0
		left 0
		z-index 999
		border-bottom 1px solid #d9d9d9
	.headTitle
		display flex
		view
			flex 1
			border-radius 8rpx
			background-color #dedede
			text-align center
			text
				color #666666
				&:first-child
					font-size 28rpx
					margin-right 8rpx
				&:last-child
					font-size 28rpx
	.headNav
		height 64rpx
		display flex
		.activNav
			width 650rpx
			height 100%
			position relative
			&>view
				padding 0 20rpx
				&:first-child
					font-size 28rpx
					color #333333
					line-height 64rpx
				&:last-child
					width 710rpx
					height 310rpx
					position absolute
					top 100%
					left 0
					display flex
					flex-flow row wrap
					justify-content space-between
					align-content space-around
					background-color #FFFFFF
					view
						width 150rpx
						height 56rpx
						// margin 0 0 40rpx 26rpx
						font-size 24rpx
						background-color #FAFAFA
						color #333
						line-height 56rpx
						text-align center
						border 1px solid #ccc
						border-radius 8rpx
						flex-shrink 0
						&.activRed
							border 1px solid #dd1a21 !important
							color #dd1a21 !important
		.headNavLeft
			width 650rpx
			height 100%
			display flex
			.headNavLeftItem
				height 100%
				flex-shrink 0
				display flex
				padding 0 40rpx 0 20rpx
				view
					flex-shrink 0
					padding 0 20rpx
					color #333333
					font-size 28rpx
					line-height 64rpx
					&.activItem
						color #dd1a21
						position relative
						&::after
							content ''
							width 100%
							height 1px
							background-color #dd1a21
							position absolute
							left 0
							bottom 0
					
		.headNavRight
			width 100rpx
			height 100%
			text-align center
			line-height 64rpx
			background-color #FFFFFF
			position relative
			view
				width 64rpx
				height 64rpx
				position absolute
				top 0
				left -64rpx
				background linear-gradient(to right, rgba(250, 255, 255, 0), rgba(250, 255, 255, 0.7), rgba(255, 255, 255, 1))
			text
				display inline-block
				font-size 40rpx
				color #666666
				transition transform 600ms
				transform rotateZ(0deg)
				&.activRotateZ
					transform rotateZ(180deg)
</style>
