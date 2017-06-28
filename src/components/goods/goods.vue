<template>
  <div class="goods">
    <div class="meau-wrapper" ref="meauWrapper">
      <ul>
        <li v-for="item,index in goods" class="menu-item" :class="{'current':currentIndex===index}"
            @click="selectMeau(index,$event)">
      <span class="text">
        <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
       </span>
        </li>
      </ul>
    </div>

    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item" v-for="food in item.foods" @click="selectFood(food,$event)">
              <div class="icon">
                <img class="img" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
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
    <shopcar :selectFoods="selectFoods"  :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcar>
    <food :food="selectedFood" ref="food"></food>
  </div>

</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopcar from '../shopcar/shopcar';
  import cartcontrol from '../cartcontrol/cartcontrol';
  import food from '../food/food';
  const ERR_OK = 0;
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: [],
        ListHeight: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.ListHeight.length; i++) {
          let height1 = this.ListHeight[i];
          let height2 = this.ListHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
          let foods = [];
          this.goods.forEach((good) => {
              good.foods.forEach((food) => {
                  if (food.count) {
                      foods.push(food);
                  }
              });
          });
        return foods;
      }
    },
    mounted() {
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          console.log(this.goods);
          this._initScroll();
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
        }
      });
      this.classMap = ['decrease', 'discount', 'guarantee', 'special', 'invoice'];
    },
    methods: {
      selectMeau(index, event) {
        if (!event._constructed) {
          return;
        }
        console.log(event);
        var foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsscroll.scrollToElement(el, 300);
      },
      selectFood(food, event) {
        if (!event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      },
      _initScroll() {
        this.meunscroll = new BScroll(this.$refs.meauWrapper, {click: true});
        this.foodsscroll = new BScroll(this.$refs.foodsWrapper, {probeType: 3, click: true});
        this.foodsscroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight() {
        var foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.ListHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.ListHeight.push(height);
        }
        console.log(this.ListHeight);
      }

    },
    components: {
      shopcar,
      cartcontrol,
      food
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .goods
    display: flex
    overflow: hidden
    position: absolute
    top: 176px
    width: 100%
    bottom: 46px
    .meau-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        display: table
        height: 54px
        width: 56px
        padding: 0 12px
        line-height: 14px
        font-size: 14px
        border-bottom: 1px solid #ddd
        &.current
          position: relative
          margin-top: 1px
          z-index 10
          background #fff
          font-weight 700px
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            background-image: url("decrease_3@3x.png")
            bg-image('decrease_3')
          &.discount
            background-image: url("discount_3@2x.png")
            bg-image('discount_3')
          &.guarantee
            background-image url("guarantee_3@2x.png")
            bg-image('guarantee_3')
          &.invoice
            background-image url("invoice_3@2x.png")
            bg-image('invoice_3')
          &.special
            background-image url("special_3@2x.png")
            bg-image('special_3')
        .text
          display: table-cell
          width: 56px
          vertical-align: middle
          border-1px(rgba(7, 17, 27, 0.1))
          font-size: 12px
    .foods-wrapper
      flex: 1
      .title
        padding-left: 14px
        height: 26px
        line-height 26px
        border-left: 2px solid #d9dde1
        font-size 12px
        color: rgb(147, 153, 159)
        background: #f3f5f7
      .food-item
        display: flex
        margin: 18px
        padding-bottom: 18px
        border-bottom: 1px solid rgba(7, 17, 27, 0.1)
        &:last-child
          margin-bottom 0
          border-bottom: none
        .icon
          width: 57px
          flex: 0 0 57px
          margin-right 10px
          .img
            width: 100%
        .content
          flex: 1
          position: relative
          .name
            margin: 2px 0 8px 0
            height 14px
            font-size 14px
            color: rgb(7, 17, 27)
          .desc
            margin-bottom: 8px
            line-height 10px
            font-size 10px
            color: rgb(147, 153, 159)
          .extra
            line-height: 10px
            margin-bottom: 8px
            line-height 10px
            font-size 10px
            .count
              margin-right 12px
          .price
            font-weight: 700
            line-height 16px
          .now
            font-size: 14px
            margin-right 8px
            color: rgb(240, 20, 20)
          .old
            text-decoration: line-through
            font-size 10px
            color: gray
          .cartcontrol-wrapper
            position: absolute
            right: 0px
            bottom: 0px


</style>
