<template>
	<div v-show="showFlag" class="food" ref="food">
		<div class="food-content">
			<div class="image-header">
				<img :src="food.image" alt="">
				<div class="back" @click="hide">
				<i class="icon-arrow-left"><</i>	
				</div>
			</div>
			<div class="content">
				<h1 class="title">{{food.name}}</h1>
				<div class="detail">
					<span class="sell-count">月售{{food.sellCount}}份</span>
					<span class="rating">好评率{{food.rating}}%</span>
				</div>
				<div class="price">
				    <span class="now">¥{{food.price}}</span>
				    <span v-if='food.oldPrice' class="old">¥{{food.oldPrice}}</span>
				</div>			
				<div class="cartcontrol-wrapper">
				  <cartcontrol :food="food"></cartcontrol>
				</div>
				<div class="buy" @click="addFirst" v-show="!food.count || food.count === 0">加入购物车</div>
			</div>
			<spilt v-show='food.info'></spilt>
			<div class="info" v-show='food.info'>
				<h1 class="title">商品信息</h1>
				<p class="text">{{food.info}}</p>
			</div>
			<spilt></spilt>
			<div class="rating">
				<h1 class="title">商品评价</h1>
				<ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
			</div>
		</div>
	</div>
</template>
<script>
const ALL = 2
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'
import Vue from 'vue'
import spilt from 'components/spilt/spilt'
import ratingselect from 'components/ratingselect/ratingselect'
	export default {
		components: {
			cartcontrol,
			spilt,
			ratingselect
		},
		props: {
			food: {
				type: Object
			}
		},
		data() {
			return {
				showFlag: false,
				selectType: ALL,
				onlyContent: true,
				desc: {
					all: '全部',
					positive: '推荐',
					negative: '吐槽'
				}
			}
		},
		methods: {
			show() {
				this.showFlag = true
				this.selectType = 0
				this.onlyContent = true
				this.$nextTick(() => {
					if (!this.scroll) {
						this.scroll = new BScroll(this.$refs.food, {
							click: true
						})
					} else {
						this.scroll.refresh()
					}
				})
			},
			hide() {
				this.showFlag = false
			},
			addFirst() {
				if (!event._constructed) {
				  return
				}
				Vue.set(this.food, 'count', 1)
			}
		}
	}
</script>

<style lang="stylus">
	.food
		position fixed
		left 0
		top 0
		bottom 48px
		z-index 30
		width 100%
		background #fff
		.food-content
			position relative
		.image-header
			position relative
			width 100%
			height 0
			padding-top 100%
			img
				position absolute
				top 0
				left 0
				width 100%
				height 100%
			.back
				position absolute
				top	10px
				left 0
				.icon-arrow-left
					display block
					padding 10px
					font-size 24px
					color #fff
		.content
			position relative
			padding 18px
			.title
				line-height 14px
				margin-bottom 8px
				font-size 14px
				font-weight 700
				color rgb(7,17,27)
			.sell-count
				margin-right 17px
			.price
				font-weight 700
				line-height 24px
				.now
					margin-right 8px
					font-size 14px
					color rgb(240,20,20)
				.old
					text-decoration line-through
					font-size 10px
					color rbg(147,153,159)
			.cartcontrol-wrapper
				position absolute
				right 12px
				bottom 12px
			.buy
				position absolute
				right 18px
				bottom 18px
				z-index 10
				height 24px
				line-height 24px
				padding 0 18px
				box-sizing border-box
				font-size 10px
				border-radius 12px
				background-color rgb(0,160,225)
				color #fff
		.info
			padding 18px
			.title
				line-height 18px
				margin-bottom 6px
				font-size 14px
				color rgb(7,17,27)
			.text
				line-height 24px
				padding 0 8px
				font-size 12px
				color #4d555d
		.rating
			padding-top 18px
			.title
				line-height 18px
				margin-left 18px 
				font-size 14px
				color rgb(7,17,27)
				

</style>