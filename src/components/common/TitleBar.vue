<template>
    <div class="title-bar"   :id="titleBarOptions.id">
      <div class="icon"><img  @mousedown.stop="" :src="titleBarOptions.iconUrl"  /></div> <!-- icon -->
				<div class="menu-title">
					<div class="menu" v-if="Object.keys(this.titleBarOptions.menu).length" ></div>   
					<div class="title">{{titleBarOptions.title}}</div>
				</div>
			<div class="status" :style="{'min-width':statusWidth}" @mousedown.stop="" >
				<div class="status-item"  @click.stop="windowToMin" v-if="getStatusButton.min"><div class="iconfont">&#xe625;</div></div>
				<div class="status-item" @click.stop="windowChangeSize" v-if="getStatusButton.max"><div class="iconfont" v-html="windowMaxIcon"></div></div>
				<div class="status-item close-window" @click.stop="windowClose" v-if="getStatusButton.closeBut"> <div class="iconfont">&#xe614;</div></div>
			</div> <!-- status -->
    </div>
</template>
<script>
import { isUndefined } from 'util';
import { setTimeout } from 'timers';
export default {
		name:"TitleBar",
		props:{
			titleBarOptions:Object,
			parentAttr:Object
		},
		data (){
			return {
				parentEl:undefined
			}
		},
		mounted (){
			this.parentEl= this.$parent.$el
		},
		watch:{
			 
		},
		data(){
			return {
				isMax:false,
				statusWidth:'3rem'
			}
		} ,
		methods:{
			windowPosition(left,top){
					this.parentEl.style.left=left+'px'
					this.parentEl.style.top=top+'px'
			}  ,
			windowChangeSize(){
				this.isMax = !this.isMax
				if(this.isMax){
						this.windowPosition(0,0)
						this.parentEl.style.width=  '100%'
			 			this.parentEl.style.height= '100%'
				}else{
						this.windowPosition(this.parentAttr.left,this.parentAttr.top)
						this.parentEl.style.width= this.parentAttr.width+ 'px'
			 			this.parentEl.style.height= this.parentAttr.height+'px'
				}
				this.$parent.$el.style.transition="width 0.2s , height 0.2s"
				setTimeout(()=>{
					this.$parent.$el.style.transition=""
				},250)
			} ,
			windowToMin(){
				 	this.parentEl.style.display="none"
			},
			windowClose(){
					this.parentEl.style.display="none"
			}
		},
		computed:{
			windowMaxIcon(){
				return this.isMax ? '&#xe611;':'&#xe62b;'
			},
			getStatusButton(){
					var buts = this.titleBarOptions
					const retBut= { min : buts.minButton && true ,max : buts.maxButton && true, closeBut : buts.closeButton && true}
					this.statusWidth = Object.values(retBut).filter(item => item).length + "rem"
				  return retBut
			} 
		} 
}
</script>

<style lang="stylus" scoped>
@import '~styles/variable.styl'
@import '~styles/mixins.styl'

.title-bar
	background:#3C3C3C
	height:.8rem
	position:relative
	display:flex
	color:#fff
	user-select:none
	.icon
		display:  flex
		min-width:.8rem
		img
			cursor:default
			width:.5rem
			padding:.1rem
			center()
	.status
		cursor:default
		min-width:3rem
		display:flex
		.status-item:hover
			background:rgba(255,255,255,0.2)
		.close-window:hover
			background:#D8351D
		.status-item
			flex:1
			display:flex
			div
				center()
	.menu-title
		flex:1
		display:flex
		justify-content:center
		width:.6rem
		.menu
			center()
			textOverflowHidden()
			padding:.1rem 0
			width:0.3rem
		.title
			width:0.3rem
			padding:.1rem 0
			textOverflowHidden()
			center()
</style>