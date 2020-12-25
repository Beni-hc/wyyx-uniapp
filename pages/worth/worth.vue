<template>
	<view class="worthBox">
		<view class="worthTop">
			<view class="worthbg">
				<image src="http://yanxuan.nosdn.127.net/0b7676e645253f84be662aacfc051922.png" mode="widthFix"></image>
				<image src="http://yanxuan.nosdn.127.net/a93a392fb8006ba26dea38477979b7b4.jpg?imageView" mode="widthFix"></image>
				<text>严选好物 用心生活</text>
			</view>
			<worthBanner :worthBannerList="worthBannerList"></worthBanner>
		</view>
		<view class="worthBottom">
			<view :class="'worthBottomHeight'+resultIndex" v-for="(resultItem,resultIndex) in resultList" :key="resultIndex" >
				<worthBottomListItem
				v-for="item in resultItem"
				:key="item.topicId"
				:worthBottomListItem="item" 
				></worthBottomListItem>
			</view>
		</view>
	</view>
</template>

<script>
	import worthBanner from "./components/worthBanner.vue"
	import worthBottomListItem from "./components/worthBottomListItem.vue"
	export default {
		data(){
			return{
				worthBannerList:[],
				resultList:[[],[]]
			}
		},
		methods:{
			_request_navWap_Api(){
				uni.request({
					url: 'http://192.168.1.2:3000/worth/navWap',
					success:(res)=>{
						let navList = res.data.data.navList
						const worthBannerList = [];
						const numb = 8;
						navList.forEach((e, index) => {
							if (index < numb) {
								worthBannerList.push([e]);
							} else {
								worthBannerList[index - numb].push(e);
							}
						});
						this.worthBannerList = worthBannerList
					}
				})
			},
			_request_recManual_Api(){
				uni.request({
					url: 'http://192.168.1.2:3000/worth/recManual',
					success:(res)=>{
						if(res.data.data.length>0){
							let topicIds = []
							let result = []
							res.data.data.forEach((list)=>{
								topicIds.push(...list.extra.detail.topicIds)
								result.push(...list.topics)
							})
							if(topicIds.length>0){
								this.exceptIds = topicIds.toString()
							} else {
								this.exceptIds = ''
							}
							this._dataMap(result)
						}
						this._request_recAuto_Api()
					}
				})
			},
			_dataMap(data=[]){
				if(data.length>0){
					let width = 345
					let resultListLeft = []
					let resultListRight = []
					let LeftHeight = 0
					let RightHeight = 0
					if(!this.resultHeight){
						this.resultHeight=[0,0]
					}
					LeftHeight = this.resultHeight[0]
					RightHeight= this.resultHeight[1]
					data.forEach((item)=>{
						let height = 0
						if(item.newAppBanner){
							height = item.newAppBannerHeight/item.newAppBannerWidth*width
							item.picUrl = item.newAppBanner
						} else {
							if(item.style === 2){
								height = width
							}else {
								height = item.appBanHeight/item.appBanWidth*width
							}
						}
						height = parseInt(height)
						item.appBanWidth = width
						item.appBanHeight = height
						item.picUrl += `?imageView&thumbnail=${width}y${height}&quality=85`
						if(item.readCount&&item.readCount > 10000){
							item.readCount = parseInt(item.readCount/1000) + "K"
						}
						if(LeftHeight <= RightHeight){
							resultListLeft.push(item)
							LeftHeight += height
						}else{
							resultListRight.push(item)
							RightHeight += height
						}
					})
					this.resultList[0].push(...resultListLeft)
					this.resultList[1].push(...resultListRight)
					this.$nextTick(()=>{
						this.resultHeight.forEach((item,index)=>{
							this._heightQuery(index)
						})
					})
				}
			},
			_request_recAuto_Api(){
				if(!this._requestRecAutoPage){
					this._requestRecAutoPage = 1
				}
				let page = this._requestRecAutoPage++
				let exceptIds = this.exceptIds
				uni.request({
					url: 'http://192.168.1.2:3000/worth/recAuto',
					data:{
						page,
						exceptIds
					},
					success:(res)=>{
						let result = []
						res.data.data.result.forEach((item)=>{
							result.push(...item.topics)
							result.push(item.look)
						})
						this._dataMap(result)
					}
				})
			},
			_heightQuery(index){
				const query = uni.createSelectorQuery().in(this)
				query.select('.worthBottomHeight'+index).boundingClientRect(boxHeight => {
					this.resultHeight[index] = boxHeight.height
				}).exec()
			}
		},
		created(){
			this._request_navWap_Api()
			this._request_recManual_Api()
		},
		onReachBottom() {
			this._request_recAuto_Api()
		},
		components:{
			worthBanner,
			worthBottomListItem
		}
	}
</script>

<style lang="stylus" scoped>
	.worthBox
		background-color #eeeeee
	.worthTop
		padding-top 144rpx
		position relative
		.worthbg
			width 100%
			position absolute
			top 0
			left 0
			text
				position absolute
				top 82rpx
				left 146rpx
				font-size 30rpx
				color #FFFFFF
			image
				display block
				&:nth-child(1)
					width 130rpx
					position absolute
					top 60rpx
					left 12rpx
				&:nth-child(2)
					width 100%
	.worthBottom
		margin 30rpx 20rpx
		display flex
		justify-content space-between
</style>