<template>
  <div class="goods">
      <div class="menu-wrapper" ref='menuwrapper'>
          <ul>
              <li v-for="(item, index) in goods" class="menu-item" :class="{'curent':curentIndex === index}" @click="selectMenu(index, $event)">
                 <span class="text border-1px"><span v-if="classMap[goods[index].type]" class="icon" :class="classMap[goods[index].type]"></span>{{item.name}}</span>
              </li>
          </ul>
      </div>
      <div class="foods-wrapper" ref='foodwrapper'>
          <ul>
              <li v-for="item in goods" class="food-list food-list-hook">
                  <h1 class="title">{{item.name}}</h1>
                  <ul>
                      <li @click="selectFood(food,$event)" v-for='food in item.foods' class="food-item border-1px">
                          <div class="icon">
                              <img :src="food.icon" width="57" height="57" alt="">
                          </div>
                          <div class="content">
                              <h2 class="name">{{food.name}}</h2>
                              <p class="desc">{{food.description}}</p>
                              <div class="extra">
                                  <span>月销{{food.sellCount}}份</span>
                                  <span>好评率{{food.rating}}%</span>
                              </div>
                              <div class="price">
                                  <span class="now">¥{{food.price}}</span>
                                  <span v-if='food.oldPrice' class="old">¥{{food.oldPrice}}</span>
                              </div>
                              <div class="cartcontrol-wrapper">
                                <cartcontrol :food="food"></cartcontrol>
                              </div>
                          </div>
                      </li>
                  </ul>
              </li>
          </ul>
      </div>    
      <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart> 
      <food :food="selectedFood" ref='food'>
        
      </food> 
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from 'components/shopcart/shopcart'
import cartcontrol from 'components/cartcontrol/cartcontrol'
import food from 'components/food/food'

const ERR_OK = 0
export default {
    components: {
      shopcart,
      cartcontrol,
      food
    },
    props: {
        seller: {
            type: Object
        }
    },
    data() {
        return {
            goods: [],
            listHeight: [],
            scrollY: 0,
            selectedFood: {}
        }
    },
    computed: {
      curentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height = this.listHeight[i]
          let height1 = this.listHeight[i + 1]
          if (!height1 || (this.scrollY >= height && this.scrollY < height1)) {
            return i
          }
        }
        return 0
      },
      selectFoods() {
        let selectFoodsList = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              selectFoodsList.push(food)
            }
          })
        })
        return selectFoodsList
      }
    },
    created() {
        this.$http.get('/api/goods').then((response) => {
            response = response.body
            if (response.errno === ERR_OK) {
                this.goods = response.data
                this.$nextTick(() => {
                    this._initScroll()
                    this._calculateHeight()
                })
            }
        })
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    methods: {
        selectMenu(index, event) {
          if (!event._constructed) {
            return
          }
          let foodList = this.$refs.foodwrapper.getElementsByClassName('food-list-hook')
          let ele = foodList[index]
          this.foodScroll.scrollToElement(ele, 300)
        },
        _initScroll() {
            this.menuScroll = new BScroll(this.$refs.menuwrapper, {
              click: true
            })
            this.foodScroll = new BScroll(this.$refs.foodwrapper, {
              click: true,
              probeType: 3
            })
            this.foodScroll.on('scroll', (pos) => {
              this.scrollY = Math.abs(Math.round(pos.y))
            })
        },
        _calculateHeight() {
            let foodList = this.$refs.foodwrapper.getElementsByClassName('food-list-hook')
            let itemHeight = 0
            this.listHeight.push(itemHeight)
            for (let i = 0; i < foodList.length; i++) {
               itemHeight += foodList[i].clientHeight
               this.listHeight.push(itemHeight)
            }
        },
        selectFood(food, event) {
          if (!event._constructed) {
            return
          }
          this.selectedFood = food
          this.$refs.food.show()
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" scoped>
@import "../../common/stylus/index.styl"
.goods
    display flex
    position absolute
    top 174px
    bottom 46px
    width 100%
    overflow hidden
    .menu-wrapper
        flex:0 0 80px
        width 80px
        background #f3f5f7
        .menu-item
          padding 0 12px
          height 54px
          width 56px
          background #f3f5f7
          display table
          &.curent
            position relative
            z-index 10
            margin-top  -1px
            background #fff
            font-weight 700
            .text
              border-none()
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
                bg-image('decrease_3')
              &.discount
                bg-image('discount_3')
              &.invoice
                bg-image('invoice_3')
              &.guarantee
                bg-image('guarantee_3')
              &.special
                bg-image('special_3')
          .text
              display table-cell
              vertical-align middle
              font-size 12px
              font-weight 200
              line-height 14px  
              border-1px(rgba(7, 17, 27, .1))
    .foods-wrapper
        flex 1
        
        .title
            font-size 12px
            color rgb(147, 153, 159)
            line-height 26px
            background #f3f5f7
            border-left 2px solid #d9dde1
            padding-left 14px
            height 26px
        .food-item
            display flex
            padding 18px 0
            margin 0 18px
            font-size 0
            border-1px(rgba(7, 17, 27, .1))
            &:last-child
                border none
        .icon
            flex 0 0 57px
            margin-right 10px
        .content
            flex 1
            .name
                font-size 14px
                color rgb(7,17,27)
                line-height 14px
                margin-top 2px
            .desc, .extra
                font-size 10px
                color rgb(147,153,159)
                line-height 10px
                margin-top 8px
            .extra 
                font-size 0
                span
                   margin-right 12px
                   font-size 10px
            .price
                color #f01414
                font-weight 700
                line-height 24px
                .now
                    margin-right 8px
                    font-size  14px
                .old
                    text-decoration line-through
                    font-size 10px
                    color rgb(147, 153, 159)
            .cartcontrol-wrapper
              position absolute
              right 0px
              bottom 12px
            
</style>
