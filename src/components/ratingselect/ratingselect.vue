<template>
	<div class="ratingselect">
		<div class="rating-type border-1px">
			<span class="block positive" :class="{'active' :selectType === 2}">{{desc.all}}<span class="count">45</span> </span>	
			<span class="block positive" :class="{'active' :selectType === 0}">{{desc.positive}}<span class="count">15</span></span>	
			<span class="block negative" :class="{'active' :selectType === 1}">{{desc.negative}}<span class="count">5</span></span>	
		</div>
		<div class="switch">
			<span class="icon-check_circle">*</span>
			<span class="text">只看内容分的评价</span>
		</div>

	</div>
</template>
<script>
	const ALL = 2
	export default {
		props: {
			ratings: {
				type: Array,
				default() {
					return []
				}
			},
			selectType: {
				type: Number,
				default: ALL
			},
			onlyContent: {
				type: Boolean,
				default: false
			},
			desc: {
				type: Object,
				default() {
					return {
						all: '全部',
						positive: '满意',
						negative: '不满意'
					}
				}
			}
		}
	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"
	.ratingselect
		.rating-type
			padding 18px 0
			margin 0 18px
			border-1px(rgba(7,17,27,0.1))
			font-size 0
			.block
				display inline-block
				font-size 12px
				padding 8px 12px
				border-radius 1px
				margin-right 8px					
				line-height 16px
				color rgb(77,85,93)
				&.active
					color #fff
				.count
					font-size 10px
					margin-left 2px
				&.positive
					background-color rgba(0,160,220,.2)
					&.active
						background-color rgb(0,160,220)
				&.negative
					background-color rgba(77,89,93,.2)
					&.active
						background-color rgb(77,89,93)
</style>