<template>
  <div class="header">
   <div class="content-wrapper">
     <div class="avatar">
       <img :src="seller.avatar" alt="" width="64" height="64"> 
     </div>
     <div class="content">
       <div class="title">
         <span class="brand"></span>
         <span class="name">{{seller.name}}</span>
       </div>
       <div class="description">
         {{seller.description}}/{{seller.deliveryTime}}分钟送达
       </div>
       <div v-if="seller.supports" class="support">
         <span class="icon" :class="classMap[seller.supports[0].type]"></span><span class="text">{{seller.supports[0].description}}</span>
       </div>
     </div>
     <div v-if="seller.supports" class="support-count" @click="showDetail">
       <span class="count">{{seller.supports.length}}个</span>
        <i class="icon iconfont icon-zuoyoujiantou"></i>
     </div>
   </div>
   <div class="bulletin-wrapper">
     <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
     <i class="icon iconfont icon-zuoyoujiantou"></i>
   </div>
   <div class="background">
       <img :src="seller.avatar" alt="" width="100%" height="100%"> 
   </div>
    <transition name="fade">
     <div v-show="detailShow" class="detail" >
       <div class="detail-wrapper clearfix">
         <div class="detail-main">
           <h1 class="name">{{seller.name}}</h1>
           <div class="star-wrapper">
             <star :size='48' :score='2.6'></star>
           </div>
           <div class="title">
             <div class="line"></div>
             <div class="text">优惠信息</div>
             <div class="line"></div>
           </div>
           <ul class="supports">
             <li class="support-item" v-for="(item, index) in seller.supports">
               <span class="icon" :class="classMap[seller.supports[index].type]"></span>
               <span class="text">{{seller.supports[index].description}}</span>
             </li>
           </ul>
           <div class="title">
             <div class="line"></div>
             <div class="text">商家公告</div>
             <div class="line"></div>
           </div>
         </div>
       </div>
       <div class="detail-close" @click="showDetail">
         <i class="iconfont icon-cha"></i>
       </div> 

     </div>
  </transition>
  </div>
</template>

<script>
import star from 'components/star/star'
export default {
  components: {
    star
  },
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail() {
      this.detailShow = !this.detailShow
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/index.styl"

.header
  color: #fff
  background: rgba(7,17,27,.5)
  position relative
  overflow hidden
  .content-wrapper
    padding: 24px 12px 18px 24px
    font-size: 0
    position relative
    .avatar
      display: inline-block
      font-size: 0
      vertical-align top
      img
        border-radius 2px
    .content
      display: inline-block
      font-size 14px
      margin-left 16px
      .title
        margin 2px 0 8px 0
        .brand
          display inline-block
          vertical-align top
          width 30px
          height 18px
          bg-image(brand)
          background-size 100%
        .name
          margin-left 6px
          font-size 16px
          line-height  18px
          font-weight 600
      .description
        margin-bottom 10px
        font-size 12px
        line-height 12px
      .support 
        font-size 0
        height 12px
        line-height 12px
        .icon
          display inline-block
          width 12px
          height 12px
          margin-right 4px
          background-size 12px 12px
          background-repeat no-repeat
          vertical-align middle
          margin-top -3px
          &.decrease
            bg-image('decrease_1')
          &.discount
            bg-image('discount_1')
          &.invoice
            bg-image('invoice_1')
          &.guarantee
            bg-image('guarantee_1')
          &.special
            bg-image('special_1')
        .text
          font-size 10px
          line-height 12px
          vertical-align middle
          display inline-block
          height 12px
    .support-count 
        position absolute
        display inline-block
        right 12px
        bottom 18px
        padding 0 8px
        height 24px
        line-height 24px
        border-radius 14px
        background rgba(0,0,0,.2)
        text-align center
        font-size 20px
      
  .bulletin-wrapper
    height 28px
    line-height 28px
    padding 0 22px 0 12px
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    position relative
    background rgba(7,17,27,.2)
    .bulletin-title
      display inline-block
      width 22px
      height 12px
      margin-top 9px
      bg-image('bulletin')
      background-size 100%
      vertical-align top
    .bulletin-text
      font-size 10px
      font-weight 200
      margin 0 4px
    .icon-zuoyoujiantou
      position: absolute;
      font-size: 18px;
      right: 6px;
      top: 1px;
  .background
    position absolute
    width 100%
    height 100%
    top 0
    left 0
    z-index -1
    filter blur(10px)
  .detail 
    position fixed
    width 100%
    height 100%
    top 0
    left 0
    overflow auto
    z-index 100
    opacity 1
    background rgba(7,17,27,.8)
    &.fade-enter-active 
      transition: all .3s ease  
    &.fade-leave-active   
       transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0)  
    &.fade-enter, &.fade-leave-to
      opacity 0
    .detail-wrapper
      min-height 100%
      width 100%
      .detail-main
        margin-top 64px
        padding-bottom 64px
        .name
          line-height 16px
          text-align center
          font-size 16px
          font-weight 700
        .star-wrapper
          margin-top  18px
          padding 2px 0
          text-align center
        .title
          display flex
          width 80%
          margin 30px auto 24px auto
          .line
            flex 1
            position relative
            top -6px
            border-bottom 1px solid rgba(255,255,255, 0.2)
          .text
            padding 0 12px
            font-size 14px
        .supports
          width 80%
          margin 0 auto
          .support-item
            padding 0 12px
            margin-bottom 12px
            font-size 0
            &:last-child
              margin-bottom 0
            .icon
              width 16px
              height  16px
              display inline-block
              vertical-align top
              margin-right 6px
              background-size 16px 16px
              background-repeat no-repeat
              &.decrease
                bg-image('decrease_2')
              &.discount
                bg-image('discount_2')
              &.invoice
                bg-image('invoice_2')
              &.guarantee
                bg-image('guarantee_2')
              &.special
                bg-image('special_2')
            .text
              font-size 12px
              line-height  14px            
    .detail-close
      positixon relative
      width 32px
      height 32px
      margin -64px auto 0 auto
      clear both
      font-size 32px
</style>
