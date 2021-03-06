<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="foodWrapper">
      <div class="food-content">
        <div class="img-header">
          <img :src="food.image">
          <div class="back"  @click="leave">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{ food.name }}</h1>
          <div class="detail">
            <span class="sell-count">月售{{ food.sellCount }}份</span>
            <span class="rating">好评率{{ food.rating }}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{ food.price }}</span><span class="old" v-show="food.oldPrice">￥{{ food.oldPrice }}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol @add="addFood" :food="food"></cartcontrol>
          </div>
          <transition name="fade">
            <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count===0">
              加入购物车
            </div>
          </transition>
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{ food.info }}</p>
        </div>
        <split></split>
        <ratinglist :ratings="food.ratings" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"></ratinglist>
      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue';
import BScroll from 'better-scroll';
import cartcontrol from 'components/cartcontrol/cartcontrol';
import split from 'components/split/split';
import ratinglist from 'components/ratinglist/ratinglist';

const ALL = 2;

export default {
  components: {
    cartcontrol,
    split,
    ratinglist
  },
  data () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    };
  },
  props: {
    food: Object
  },
  methods: {
    show() {
      this.showFlag = true;
      this.selectType = ALL;
      this.onlyContent = true;
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.foodWrapper, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      });
    },
    leave () {
      this.showFlag = false;
    },
    addFirst (event) {
      this.$emit('add', event.target);
      Vue.set(this.food, 'count', 1);
    },
    addFood(target) {
      this.$emit('add', target);
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
.food
  position: fixed
  left: 0
  top: 0
  bottom: 48px
  z-index: 30
  width: 100%
  background: #fff
  transform: translate3d(0, 0, 0)
  &.move-enter-active, &.move-leave-active
    transition: all 0.3s linear
  &.move-enter, &.move-leave-active
    transform: translate3d(100%, 0, 0)
  .img-header
    position: relative
    width: 100%
    height: 0
    padding-top: 100%
    img
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
    .back
      position: absolute
      top: 10px
      left: 0
      .icon-arrow_lift
        display: block
        padding: 10px
        font-size: 20px
        color: #fff
  .content
    position: relative
    padding: 18px
    .title
      line-height: 14px
      margin-bottom: 8px
      font-size: 14px
      font-weight: 700
      color: rgb(7,17,27)
    .detail
      margin-bottom: 18px
      line-height: 10px
      height: 10px
      font-size: 0px
      .sell-count, .rating
        font-size: 10px
        color: rgb(147,152,159)
      .sell-count
        margin-right: 12px
    .price
      font-weight: 700
      line-height: 24px
      .now
        margin-right: 8px
        font-size: 14px
        color: rgb(240,20,20)
      .old
        text-decoration: line-through
        font-size: 10px
        color: rgb(147, 153, 159)
    .cartcontrol-wrapper
      position: absolute
      right: 12px
      bottom: 12px
    .buy
      position: absolute
      right: 18px
      bottom: 18px
      color: #fff
      z-index: 10
      height: 24px
      line-height: 24px
      padding: 0 12px
      box-sizing: border-box
      border-radius: 12px
      font-size: 10px
      background: rgb(0,160,220)
      opacity: 1
      &.fade-enter-active, &.fade-leave-active
        transition: all 0.2s
      &.fade-enter, &.fade-leave-active
        opacity: 0
        z-index: -1
  .info
    padding: 18px
    .title
      line-height: 14px
      margin-bottom :6px
      font-size: 14px
      color: rgb(7,17,27)
    .text
      line-height: 24px
      font-size: 12px
      padding: 0 8px
      color: rgb(77,85,93)
</style>
