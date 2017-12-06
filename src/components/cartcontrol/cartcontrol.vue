<template>
	<div class="cartcontrol">
	   <transition name="roll">
		<div class="cart-decrease" v-show='food.count' @click.stop.prevent='decreaseCart'>
		<span class="inner">-</span>
		</div>
		</transition>
		<div class="cart-count" v-show='food.count > 0'>{{food.count}}</div>
		<div class="cart-add" @click.stop.prevent='addCart'>+</div>
	</div>
</template>
<script>
    import Vue from 'vue'
	export default {
		data() {
			return {

			}
		},
		props: {
			food: {
				type: Object
			}
		},
		methods: {
			addCart(event) {
				if (!event._constructed) {
					return
				}
				if (!this.food.count) {
					Vue.set(this.food, 'count', 1)
					this.food.count = 1
				} else {
					this.food.count ++
				}
				console.log(this.food.count)
			},
			decreaseCart() {
				if (!event._constructed) {
					return
				}
				if (this.food.count) {
					this.food.count --
				}
			}
		}
	}
</script>
<style lang="stylus" rel="stylesheet">
	.cartcontrol
		font-size 0
		.cart-decrease	
			display inline-block
			transform translate3D(0,0,0)
			.inner			
				display inline-block
				padding 6px
				font-size 24px
				line-height 24px
				border 1px solid #00a0dc
				border-radius 50%
				width	14px
				height 14px
				line-height 14px
				text-align center
				color #00a0dc			
				transform rotate(0)
			&.roll-leave-to,&.roll-enter
				opacity  0
				transform translate3D(50px,0,0)
			&.roll-leave-to,&.roll-enter
				.inner
					transform rotate(180deg)
			&.roll-enter-active,&.roll-leave-active
				transition all .5s ease
			&.roll-enter-active,&.roll-leave-active
				.inner
					transition all .4s ease
		.cart-count
			display inline-block
			font-size 12px
			vertical-align top
			width 12px
			padding 0px 5px 0
			line-height 24px
			text-align center 
		.cart-add
			display inline-block
			padding 6px
			font-size 24px
			line-height 24px
			border 1px solid #00a0dc
			border-radius 50%
			width	14px
			height 14px
			text-align center
			line-height 14px
			color #00a0dc
			
		
</style>