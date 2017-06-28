<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
       <div class="title">
         <span class="brand"></span>
         <span class="name">{{seller.name}}</span>
       </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="supports" v-if="seller.supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showdetail">
        <span class="count" >{{seller.supports.length}}个</span>
        <i class="icon"></i>
      </div>
    </div>
    <div class="bulletion-wrapp" @click="showdetail">
      <span class="bulletion-title"></span><span class="bulletion-text">{{seller.bulletin}}</span><i class="icon"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar"  width="100%" height="100%">
    </div>
    <div class="detail" v-show="datailShow">
      <div class="deail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
           <star :size="48" :score="seller.score" ></star>
            </div>
           <div class="title">
             <div class="line"></div>
             <div class="text">优惠信息</div>
             <div class="line"></div>
           </div>
            <ul v-if="seller.supports" class="suppo">
              <li class="supports-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[seller.supports[1].type]"></span>
                <span class="text">{{seller.supports[1].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>

          </div>

      </div>
      <div class="detail-close" @click="hiddendetail">×</div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from '../star/star.vue';
  export default {
      props: {
          seller: {
              type: Object
          }
      },
     data() {
          return {
            datailShow: false
          };
     },
     methods: {
       showdetail() {
           this.datailShow = true;
       },
       hiddendetail() {
         this.datailShow = false;
       }
     },
     created() {
          this.classMap = ['decrease', 'discount', 'guarantee', 'special', 'invoice'];
     },
    components: {
      star
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import  '../../common/stylus/mixthin';

  .header
    color: #ffffff
    background :rgba(7,17,27,0.5)
    overflow: hidden
    position: relative
    .content-wrapper
     padding :24px 12px 18px 24px
     font-size :0
     position: relative
     .avatar
      display :inline-block
      font-size :14px
      vertical-align top
     img
      border-radius:2px
     .content
       display :inline-block
       font-size 14px
       margin-left 16px
       .title
        margin :2px 0 8px 0
        .brand
          width: 30px
          height: 18px
          display :inline-block
          bg-image('brand')
          background-image:url("brand@2x.png")
          background-size:30px 18px
          background-repeat:no-repeat
          vertical-align: top
          .name
            margin-left 6px
            font-size:16px
            font-weight:bold
            line-height 18px
     .description
       margin-bottom: 10px
       line-height 12px
       font-size 12px
     .supports
       .icon
         display:inline-block
         width: 12px
         height: 12px
         margin-right 4px
         background-size:12px 12px
         vertical-align:middle
         background-repeat:no-repeat
         &.decrease
              background-image:url("decrease_1@2x.png")
         &.discount
               background:url("discount_1@2x.png")
         &.guarantee
              background-image:url("guarantee_1@2x.png")
         &.special
              background:url("special_1@2x.png")
         &.invoice
             background-image:url("invoice_1@2x.png")
       .text
          line-height: 12px
          font-size:10px
     .support-count
      position: absolute
      bottom: 18px
      right: 12px
      padding:0 8px
      height: 24px
      line-height 24px
      border-radius 14px
      background-color:rgba(0,0,0,0.2)
      text-align:center
      .count
        font-size: 10px
        vertical-align:top

      .icon
        line-height:24px
        margin-left: 2px
        font-size: 10px



    .bulletion-wrapp
      height: 28px
      line-height 28px
      padding:0 22px 0 12px
      white-space:nowrap
      overflow: hidden
      text-overflow:ellipsis
      .bulletion-title
        display: inline-block
        width: 22px
        height: 12px
        vertical-align:top
        margin-top:10px
        background-image:url("brand@2x.png")
        background-size 22px 12px
        background-repeat :no-repeat
      .bulletion-text
        font-size: 10px
        margin-left 4px

    .background
      position: absolute
      top:0
      left:0
      width:100%
      height:100%
      z-index:-1
      filter:blur(10px)
    .detail
      position:fixed
      top:0
      left:0
      width:100%
      height:100%
      overflow: auto
      transition :all 0.5s
      background rgba(7,17,27,0.8)
      z-index 1000
      .clearfix
        display: inline-block
        &:after
          display: block
          content: '.'
          height:0
          line-height 0px
          clear: both
          visibility:hidden
      .deail-wrapper
        min-height:100%
        width:100%
        .detail-main
          margin-top: 64px
          padding-bottom:64px
          .name
            line-height 16px
            text-align: center
            font-size:16px
          .star-wrapper
            margin-top 18px
            padding:2px 0
            text-align:center
          .title
            display: flex
            width:80%
            margin :30px auto 24px auto
            .line
              flex:1
              position:relative
              top: -11px
              border-bottom:1px solid rgba(255,255,255,0.8)
            .text
              font-size:24px
              padding: 0 12px
          .suppo
              width:80%
              margin:0 auto
              .supports-item
                 padding:0 12px
                 margin-bottom 12px
                 font-size:0
                 &:last-child
                    margin-bottom 0px
                 .icon
                      display:inline-block
                      width:16px
                      height:16px
                      margin-right:6px
                      vertical-align:top
                      background-size:16px 16px!important
                      background-repeat:no-repeat
                      &.decrease
                        background-image:url("decrease_1@2x.png")
                      &.discount
                        background:url("discount_1@2x.png")
                      &.guarantee
                        background-image:url("guarantee_1@2x.png")
                      &.special
                        background:url("special_1@2x.png")
                      &.invoice
                        background-image:url("invoice_1@2x.png")
                 .text
                    line-height 16px
                    font-size:10px
          .bulletin
            width:80%
            margin:0 auto
            .content
              padding:0 12px
              line-height 24px
              font-size:12px
      .detail-close
        position: relative
        clear:both
        font-size 32px
        margin:-64px auto 0 auto
        width 32px
        height: 32px
        text-align: center;
</style>
