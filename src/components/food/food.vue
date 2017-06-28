<template>
<div v-show="showFlag" class="food" ref="foodHeight">
<div class="food-content">
  <div class="image-title">
  <img :src="food.image" alt="">
  <div class="back" @click="hide"> < </div>
</div>
  <div class="content">
    <h1 class="title">{{food.name}}</h1>
    <div class="detail">
      <span class="sell-count">月售:{{food.sellCount}}份</span>
      <span class="rating">好评率:{{food.rating}}%</span>
    </div>
    <div class="price">
      <span class="new">¥{{food.price}}</span>
      <span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
    </div>

    <div class="cartcontrol-wrapper">
      <cartcontrol :food="food"></cartcontrol>
    </div>
    <div @click.stop="addFirst(food,$event)" class="buy" v-show="!food.count || food.count==0">
      加入购物车
        </div>
  </div>
   <split v-show="food.info"></split>
  <div class="infor" v-show="food.info">
    <h1 class="title">商品信息</h1>
    <div class="text">{{food.info}}</div>
  </div>
  <split v-show="food.ratings"></split>
  <div class="rating">
    <h1 class="title">商品评价</h1>
    <ratingselect @select="selectRating" @toggle="toggleContent" :ratings="food.ratings" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"></ratingselect>

  <div class="rating-wrapper">
    <ul class="" v-show="food.ratings && food.ratings.length">
    <li v-show="needShow(rating.rateType,rating.text)" class="rating-item" v-for="rating in food.ratings">
      <div class="user">
        <span class="name">{{rating.username}}</span>
        <img :src="rating.avatar" class="avatar" width="12" height="12" alt="">
      </div>
      <div class="time">{{rating.rateTime | formDate}}</div>
      <p class="text">
        <span v-show="rating.rateType===0"  class="icon">√</span>
        <span v-show="rating.rateType===1"  class="icon1">×</span>
        {{rating.text}}

       </p>
    </li>
    </ul>

      <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
        暂无评价
      </div>

  </div>
  </div>
</div>

</div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol';
  import split from '../split/split';
  import {formDate} from './../../common/js/data';
  import ratingselect from '../ratingselect/ratingselect';
  import Vue from 'vue';
  //  const POSITIVE = 0;
  //  const NEGATIVE = 1;
      const ALL = 2;
export default{
    props: {
      food: {
        type: Object
      }
    },
  data() {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: false,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
  },
  mounted() {
  },
  methods: {
        show() {
         this.showFlag = true;
         this.selectType = ALL;
         this.onlyContent = false;
         this.$nextTick(() => {
             if (!this.scroll) {
                 this.scroll = new BScroll(this.$refs.foodHeight, {
                     click: true
                 });
             } else {
                 this.scroll.refresh();
             }
         });
        },
      hide() {
         this.showFlag = false;
      },
    needShow(type, text) {
//            没有内容的时候
      if (this.onlyContent && !text) {
        return false;
      }
      if (this.selectType === ALL) {
        return true;
      } else {
        return type === this.selectType;
      }
    },
    addFirst(food, event) {
      if (!event._constructed) {
        return;
      }
      Vue.set(this.food, 'count', 1);
    },
  selectRating(type) {
    this.selectType = type;
    this.$nextTick(() => {
      this.scroll.refresh();
    });
  },
  toggleContent() {
    this.onlyContent = !this.onlyContent;
    console.log(this.onlyContent);
    this.$nextTick(() => {
      this.scroll.refresh();
    });
  }
  },
  filters: {
    formDate(time) {
        let date = new Date(time);
        return formDate(date, 'yyyy-MM-dd hh:mm');
    }
  },
  components: {
    cartcontrol,
    split,
    ratingselect
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food
    position:fixed
    left:0
    bottom 48px
    top:0
    z-index:30
    width:100%
    background:#fff
    .food-content
       position:relative
      .image-title
         position :relative
         width:100%
         height :0
         padding-top :100%
         img
            position :absolute
            top:0
            left :0
            width 100%
            height:100%
         .back
             position :absolute
             top:10px
             left:0
             font-size:20px
             padding :10px
             color:#fff
      .content
          padding:18px
          position :relative
          .title
            line-height: 14px
            margin-bottom 8px
            font-size: 14px
            font-weight 700px
            color :rgb(7,17,27)
          .detail
             margin-bottom 18px
             line-height 10px
             font-size 0
             height:10px
             .sell-count,.rating
               display :inline-block
               font-size: 10px
               color :rgb(147,153,159)
             .sell-count
                margin-right 12px
          .price
              font-weight :700
              line-height :24px
              font-size:0
              .new
                 margin-right 8px
                 font-size: 14px
                 color:rgb(240,20,20)
              .old
                  text-decoration:line-through
                  font-size 10px
                  color:rgb(147,153,159)
          .cartcontrol-wrapper
              position :absolute
              right: 18px
              bottom:18px
          .buy
            position :absolute
            right: 18px
            bottom:18px
            z-index 10px
            line-height: 24px
            height 24px
            padding 0 12px
            box-sizing border-box
            font-size 10px
            border-radius 12px
            color #fff
            background rgb(0,160,220)

      .infor
         padding :18px
         .title
           line-height :14px
           margin-bottom 16px
           font-size :14px
           color: rgb(7,17,27)
         .text
           line-height :24px
           font-size: 12px
           padding:0 8px
           color: rgb(77,85,93)

      .rating
         padding-top :18px
         .title
           margin-left 18px
           line-height 14px
           font-size: 14px
           color:rgb(7,17,27)
         .rating-wrapper
            padding 0 18px
            .rating-item
              position relative
              padding:16px 0
              border-bottom:1px solid rgba(7,17,27,0.3)
              &.last-child
                border-bottom:none
              .user
                position absolute
                right:0
                top: 16px
                line-height 12px
                font-size: 10px
                color :rgb(147,153,159)
                .name
                  display inline-block
                  vertical-align top
                  font-size 10px
                  margin-right 6px
                  color: rgb(147,153,159)
                .avatar
                   border-radius 50%
              .time
                margin-bottom 6px
                line-height 12px
                font-size: 10px
                color :rgb(147,153,159)
              .text
                line-height 16px
                font-size: 12px
                color:rgb(7,17,27)
                .icon,.icon1
                   margin-right 4px
                   line-height 24px
                   font-size 12px
                .icon
                   content :"1"
                   color:rgb(0,160,220)
                .icon1
                  content :"1"
                  color:rgb(147,153,159)


            .no-rating
              padding 16px
              font-size 12px
              color:rgb(147,153,159)
</style>
