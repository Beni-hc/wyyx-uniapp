<template>
	<image class="imageObserver" :class="showCss" :src="activSrc?src:''" :mode="mode" @load="load"></image>
</template>

<script>
	export default {
		props:{
			src:{
				type:String,
				default: ""
			},
			mode:{
				type:String,
				default: "widthFix"
			},
			options:{
				type:Object,
				default:function(){
					return {}
				}
			},
			selector:{
				type:String,
				default: ""
			},
			margins:{
				type:Object,
				default:function(){
					return {}
				}
			},
			isLoad:{
				type:Boolean,
				default:false
			}
		},
		data(){
			return {
				activSrc: false,
				showCss: ""
			}
		},
		methods:{
			load(){
				if(this.showCss){
					this.showCss = ""
				}
				if(this.isLoad){
					this.$emit("load")
				}
			},
			observerCallBack(res){
				if(res.intersectionRatio>0){
					this.showCss = "imageCss"
					this.$nextTick(function(){
						this.activSrc = true
						if(this.observer){
							this.observer.disconnect()
						}
						this.observer = null
					})
				}
			}
		},
		onReady() {
			this.observer = uni.createIntersectionObserver(this,this.options)
			if(this.selector){
				this.observer.relativeTo(this.selector,this.margins).observe('.imageObserver',this.observerCallBack)
			} else {
				this.observer.relativeToViewport(this.margins).observe('.imageObserver',this.observerCallBack)
			}
		},
		onUnload() {
			if (this.observer) {
				this.observer.disconnect()
			}
		},
	}
</script>

<style lang="stylus" scoped>
	.imageObserver
		width 100%
		height 100%
		min-height 2px
	.imageCss
		will-change transform
</style>
