<template>
	<view class="categoryBox">
		<view class="categoryLeft">
			<movable-area class="categoryLeftSlider">
				<movable-view class="categoryLeftSliderItem" direction="vertical" :y="y" disabled="true">
				</movable-view>
			</movable-area>
			<view class="categoryLeftItem" 
			:class="{categoryLeftItemActiv:categoryLeftActivRed === categoryL1ListIndex}"
			v-for="(categoryL1ListItem,categoryL1ListIndex) in categoryL1List" 
			:key="categoryL1ListItem.id" 
			@click="clickActivItem(categoryL1ListIndex,categoryL1ListItem.id,$event)"
			>
				{{categoryL1ListItem.name}}
			</view>
		</view>
		<scroll-view class="categoryRight" scroll-x="true">
			<view>
				<categoryItem
				:categoryGroupList="categoryGroupList" 
				:bannerList="bannerList"
				></categoryItem>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	import categoryItem from './components/categoryItem.vue'
	export default {
		data(){
			return {
				categoryL1List:[],
				categoryGroupList:[],
				bannerList:[],
				categoryLeftActivRed: 0,
				y:20
			}
		},
		methods:{
			_requestApi_category(ID=""){
				uni.showNavigationBarLoading()
				uni.request({
					url: 'http://192.168.1.2:3000/category',
					data: {
						ID
					},
					success:(res)=>{
						if(!ID){
							this.categoryL1List = res.data.data.categoryL1List
						}
						this.categoryGroupList = res.data.data.categoryGroupList
						this.bannerList = res.data.data.currentCategory.bannerList
						uni.hideNavigationBarLoading()
					}
				})
			},
			clickActivItem(index,id,e){
				this.categoryLeftActivRed = index
				this.y = e.currentTarget.offsetTop
				this._requestApi_category(id)
			}
		},
		created() {
			this._requestApi_category()
		},
		components:{
			categoryItem
		}
	}
</script>

<style lang="stylus" scoped>
	.categoryBox
		height 100vh
		display flex
		position relative
		background-color #FFFFFF
		&::before
			content ""
			height 1px
			position absolute
			top 0
			left 0
			right 0
			background-color rgba(0,0,0,.15)
			z-index 999
		.categoryLeft
			width 160rpx
			height 100%
			flex-shrink 0
			position relative
			border-right 1px solid rgba(0,0,0,.15)
			.categoryLeftSlider
				height auto
				width 6rpx
				position absolute
				top 0
				left 0
				bottom 0
				.categoryLeftSliderItem
					width 6rpx
					height 25px
					background-color #ab2b2b
			.categoryLeftItem
				height 25px
				font-size 28rpx
				color #333333
				text-align center
				line-height 25px
				margin-top 20px
			.categoryLeftItemActiv
				color #ab2b2b
		.categoryRight
			width 100vh
			height 588rpx
			flex-shrink 0
			flex-grow 0
			transform-origin left bottom
			transform translateY(-100%) rotateZ(90deg)
			view
				width 588rpx
				height 100vh
				transform-origin top right
				transform translateX(-100%) rotate(-90deg)

</style>