<template>
<div class="ratingselect">
  <div class="ratingtype">
    <span class="block positive" @click="select(2,$event)" :class="{'active':selectType===2}">
      {{desc.all}}  <span class="count">{{ratings.length}}</span>
    </span>
    <span class="block positive"  @click="select(0,$event)"  :class="{'active':selectType===0}">
      {{desc.positive}} <span class="count">{{positive.length}}</span>
    </span>
    <span class="block negative" @click="select(1,$event)"  :class="{'active':selectType===1}">
      {{desc.negative}} <span class="count">{{negative.length}}</span>
    </span>

  </div>
  <div class="switch">
    <label for="only">
      <input @click="toggleContent($event)" type="checkbox" id="only"  class="check" >
      只看有内容的评价
    </label>
  </div>
</div>
</template>
<script type="text/ecmascript-6">
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;
export default {
  props: {
   ratings: {
       type: Array,
       default() {
           return [];
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
        };
      }
    }
  },
  computed: {
      positive() {
          return this.ratings.filter((ratings) => {
              return ratings.rateType === POSITIVE;
          });
      },
    negative() {
      return this.ratings.filter((ratings) => {
        return ratings.rateType === NEGATIVE;
      });
    }
  },
  methods: {
    select(type, event) {
      if (!event._constructed) {
        return;
      }
      this.$emit('select', type);
    },
    toggleContent(event) {
      this.$emit('toggle');
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratingselect
     .ratingtype
       font-size:0
       padding :18px 0
       margin :0 18px
       border-bottom 1px solid rgba(7,17,27,0.2)
       .block
         font-size: 12px
         padding:8px 12px
         line-height 16px
         border-radius:1px
         margin-right 8px
         display :inline-block
         color:rgb(77,85,93)
         &.active
           color: #fff
         .count
           font-size 8px
           margin-left:2px
         &.positive
           background :rgba(0,160,220,0.2)
           &.active
             background :rgba(0,160,220,1)
         &.negative
           background :rgba(77,85,93,0.2)
           &.active
             background :rgba(77,85,93,1)

     .switch
       padding 12px 18px
       color:rgb(147,153,159)
       font-size: 12px
       line-height 24px
       border-bottom:1px solid rgba(7,17,27,0.2)
</style>
