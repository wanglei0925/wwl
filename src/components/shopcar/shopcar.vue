<template>
  <div>
  <div class="shopcar">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo-content" :class="{'highlight':totalCount>0}">
            <span class="icon-car" :class="{'highlight':totalCount>0}">车</span>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalCount>0}">
          ${{totalPrice}}
        </div>
        <div class="desc">
          另需配送费{{deliveryPrice}}元

        </div>
      </div>
      <div class="content-right" :class="payClass" @click.stop="pay">
        <div class="pay">
          {{payDesc}}
        </div>
      </div>
 </div>
    <!--购物车的列表-->
    <div class="shopcar-list" v-show="listShow">
     <div class="list-header">
       <h1 class="title">购物车</h1>
       <span class="empty" @click="empty">清空</span>
     </div>
      <div class="list-content" ref="listContent">
        <ul>
          <li class="food" v-for="food in selectFoods">
            <span class="name">{{food.name}}</span>
            <div class="price">
              <span>¥{{food.price*food.count}}</span>
            </div>
            <div class="control-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="list-mask" @click="hidelist" v-show="listShow"></div>
</div>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol';
  export default{
    props: {
        selectFoods: {
          type: Array,
          default() {
              return [
                {
                  price: 10,
                  count: 5
                }
              ];
          }
        },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        fold: true
      };
    },
    computed: {
        totalPrice() {
            let total = 0;
            this.selectFoods.forEach((food) => {
                total += food.price * food.count;
            });
            return total;
        },
      totalCount() {
            let count = 0;
        this.selectFoods.forEach((food) => {
            count += food.count;
        });
        return count;
      },
      payDesc() {
           if (this.totalPrice === 0) {
             return `￥${this.minPrice}元起送`;
           } else if (this.totalPrice < this.minPrice) {
               let diff = this.minPrice - this.totalPrice;
               return `还差${diff}元起送`;
           } else {
             return '去结算';
           }
      },
      payClass() {
        if (this.totalPrice < this.minPrice) {
            return 'noenough';
        } else {
            return 'enough';
        }
      },
      listShow() {
            if (!this.totalCount) {
                this.fold = true;
                return false;
            }
            let show = !this.fold;
            if (show) {
              this.$nextTick(() => {
                  if (!this.scroll) {
                    this.scroll = new BScroll(this.$refs.listContent, {
                      click: true
                    });
                  } else {
                      this.scroll.refresh();
                  }
              });
            }
            return show;
      }
    },
    methods: {
      toggleList() {
        if (!this.totalCount) {
            return;
        }
        this.fold = !this.fold;
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      },
      pay() {
        if (this.totalPrice < this.minPrice) {
            return;
        }
       window.alert(`支付${this.totalPrice}元`);
      },
      hidelist() {
          this.fold = true;
      }
    },
    components: {
      cartcontrol
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .shopcar
    position: fixed
    bottom: 0
    left: 0
    height: 48px
    width: 100%
    z-index: 50
    .content
      height: 48px
      display: flex
      background: #141d27
      position relative
      z-index 50
      .content-left
        flex: 1
        font-size: 0
        .logo-wrapper
          display: inline-block
          position: relative
          z-index 50
          top: -10px;
          padding: 6px
          margin: 0 18px
          width: 56px
          height 56px
          box-sizing: border-box
          background #141d27
          border-radius: 50%
          vertical-align: top
          .logo-content
            width: 100%
            border-radius: 50%
            background: #24343c
            text-align: center
            &.highlight
               background :rgb(0,160,220)
            .icon-car
              line-height: 44px
              font-size 22px
              color: #80858a
              &.highlight
                 color:#fff
          .num
            position:absolute
            top:0
            right:0
            width:24px
            height :16px
            line-height :16px
            text-align :center
            border-radius:16px
            font-size :9px
            font-weight:700
            color #fff
            background :rgb(240,20,20)
            box-shadow :0 4px 8px 0 rgba(0,0,0,0.4)
        .price
          display: inline-block
          line-height: 24px
          color: #80858a
          margin-top: 12px
          font-size: 16px
          font-weight: 700
          box-sizing border-box
          padding-right: 12px
          border-right: 1px solid rgba(255, 255, 255, 0.1)
          vertical-align: top
          &.highlight
             color :#fff
        .desc
          display: inline-block
          margin-left: 12px
          font-size: 14px
          margin-top 12px
          line-height 24px
          color: rgba(255, 255, 255, 0.4)
          vertical-align: top
      .content-right
        flex: 0 0 105px
        width: 105px
        &.noenough
          background #2b333b
        &.enough
           color: rgba(255, 255, 255,1)
           background :#00b43c
        .pay
           color: rgba(255, 255, 255, 1)
           line-height :48px
           font-size: 12px
           font-weight:700
           text-align :center
           height :48px



    .shopcar-list
      position:absolute
      left:0
      z-index:45
      width: 100%
      top:0
      transform:translate(0,-100%);
      .list-header
         height :40px
         line-height 40px
         background :#f3f5f7
         padding :0 18px
         .title
            float: left
            font-size: 14px
            color: rgb(7,17,27)
         .empty
             float:right
             font-size :12px
             color :rgb(0,160,220)
      .list-content
          padding 0 18px
          overflow: hidden
          background:#fff
          max-height: 217px
          .food
            position :relative
            padding 12px 0
            box-sizing: border-box
            border-bottom 1px solid rgba(7,17,27,0.1)
            .name
               line-height :24px
               font-size: 14px
               color :rgb(7,17,27)
            .price
                position absolute
                bottom :12px
                line-height:24px
                font-size :14px
                font-weight: 700
                color rgb(240,20,20)
                right :90px
            .control-wrapper
                 position absolute
                 right:0
                 bottom:16px

    .list-mask
      position fixed
      top:0
      left :0
      width:100%
      height 100%
      z-index :40
      backdrop-filter :blur(10px)
      background :rgba(0,0,0,0.4)
</style>
