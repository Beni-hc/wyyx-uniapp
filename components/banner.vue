<template>
	<view class="bannerBox">
		<swiper class="swiperStyle" :autoplay="autoplay" :interval="3000" :duration="500" circular="true" @change="dotIndex">
			<template v-for="(item,index) in bannerList" >
				<swiper-item :key="item.id" >
					<view class="imageBox" :style="bannerStyle">
						<image
						class="imageCss"
						:src="isShowImage[index]? item.picUrl:''" 
						mode="widthFix" 
						></image>
					</view>
				</swiper-item>
			</template>
		</swiper>
		<view class="swiperDot">
			<view 
			class="iconfont" 
			:class="dotsIndex===index?'icon-douhao':'icon-dianxuan'" 
			v-for="(itemIcon,index) in bannerList" 
			:key="itemIcon.id"
			>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			bannerList:{
				type:Array,
				default:function(){
					return []
				}
			},
			bannerStyle:{
				type:String,
			}
		},
		data(){
			return{
				dotsIndex: 0,
				autoplay:true,
			}
		},
		methods:{
			dotIndex(e){
				this.dotsIndex = e.detail.current	
			}
		},
		computed:{
			isShowImage(){
				if(!this.isShowImageX){
					this.isShowImageX = []
				}
				let maxIndex = this.bannerList.length && this.bannerList.length - 1
				let activImage = this.dotsIndex
				let leftImage = activImage === 0 ? maxIndex : activImage - 1
				let rightImage = activImage === maxIndex ? 0 : activImage + 1
				if(!this.isShowImageX[activImage]){
					this.isShowImageX[activImage] = true
				}
				if(!this.isShowImageX[leftImage]){
					this.isShowImageX[leftImage] = true
				}
				if(!this.isShowImageX[rightImage]){
					this.isShowImageX[rightImage] = true
				}
				return this.isShowImageX
			}
		},
		onReady() {
			this.observer = uni.createIntersectionObserver(this);
			this.observer.relativeToViewport().observe('.bannerBox', (res) => {
				if(res.intersectionRatio > 0){
					this.autoplay = true
				}else {
					this.autoplay = false
				}
			})
		},
		onUnload() {
			if (this.observer) {
				this.observer.disconnect()
			}
		}
	}
</script>

<style lang="stylus" scoped>
	.bannerBox
		width 100%
		height 100%
		position relative
		.swiperStyle
			width: 100%
			height 100%
		.imageBox 
			height 100%
			box-sizing border-box
			position relative
			overflow hidden
			.imageCss
				width 100%
				position absolute
				top 50%
				left 0
				transform translateY(-50%)
	.swiperDot
		position absolute
		left 0
		bottom 10rpx
		right 0
		display flex
		justify-content center
		transform translate(0%,0px) translateZ(0px)
		.icon-douhao
			color #d81e06
			font-size 22rpx
			margin 0 3rpx
		.icon-dianxuan
			color #FFFFFF
			font-size 16rpx
			margin 0 6rpx
</style>
