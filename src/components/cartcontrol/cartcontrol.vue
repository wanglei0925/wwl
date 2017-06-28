<template>
<div class="cartcontrol">
  <div class="cart-decrease" v-show="food.count>0" @click.stop="decCart($event)">-</div>
  <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
  <div class="cart-add" @click.stop="addCart($event)">+</div>
</div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue';
 export default{
     props: {
         food: {
             type: Object
         }
     },
   methods: {
     addCart(event) {
       if (!event._constructed) {
         return;
       }
       if (!this.food.count) {
         Vue.set(this.food, 'count', 1);
       } else {
         this.food.count++;
       }
     },
     decCart(event) {
       if (!event._constructed) {
         return;
       }
       if (this.food.count) {
         this.food.count--;
       }
     }
   }
 };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
     font-size:0
    .cart-decrease
        display :inline-block
        vertical-align :top
        line-height:20px
        width :18px
        height :18px
        font-size :14px
        text-align :center
        color :#fff
        border-radius:50%
        background :rgb(0,160,220)
    .cart-count
        display :inline-block
        line-height:18px
        font-size :12px
        margin :0 4px
        color :rgb(147,157,167)
      .cart-add
        display :inline-block
        vertical-align :top
        line-height:18px
        width :18px
        height :18px
        text-align :center
        color:#fff
        border-radius:50%
        font-size :14px
        background :rgb(0,160,220)

</style>
