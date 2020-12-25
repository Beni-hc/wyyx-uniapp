<template>
	<view class="">
		<homeHead :searchData="searchData"></homeHead>
		<homeBody 
		:bannerList = "bannerList" 
		:policyDescList = "policyDescList" 
		:kingKongList = "kingKongList" 
		:floorList = "floorList" 
		:indexActivityModule = "indexActivityModule" 
		:categoryHotSellModule = "categoryHotSellModule" 
		:newItemList = "newItemList" 
		:sceneLightShoppingGuideModule = "sceneLightShoppingGuideModule" 
		></homeBody>
	</view>
</template>

<script>
	import homeHead from './components/homeHead.vue'
	import homeBody from './components/homeBody.vue'
	export default {
		data(){
			return {
				searchData: 0,
				bannerList:[],
				policyDescList:[],
				kingKongList:[],
				floorList:[],
				indexActivityModule:[],
				categoryHotSellModule:{},
				newItemList:[],
				sceneLightShoppingGuideModule:[]
			}
		},
		methods:{
			_requestApi(){
				uni.request({
					url: 'http://192.168.1.2:3000/home',
					success:(res)=>{
						this.bannerList = res.data.data.data.focusList
						this.policyDescList =res.data.data.data.policyDescList
						this.kingKongList = res.data.data.data.kingKongModule.kingKongList
						this.floorList = res.data.data.data.bigPromotionModule.floorList
						this.$nextTick(function(){
							this.indexActivityModule = res.data.data.data.indexActivityModule
							this.categoryHotSellModule = res.data.data.data.categoryHotSellModule
							this.newItemList = res.data.data.data.newItemList.slice(0,6)
							this.sceneLightShoppingGuideModule = res.data.data.data.sceneLightShoppingGuideModule
						})
					}
				})
				
				uni.request({
					url: 'http://192.168.1.2:3000/search',
					success:(res)=>{
						this.searchData = res.data.data
					}
				})
			}
		},
		created() {
			this._requestApi()
		},
		components:{
			homeHead,
			homeBody
		}
	}
</script>

<style lang="stylus" scoped>
	
</style>
