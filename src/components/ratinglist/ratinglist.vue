<template>
  <div class="ratinglist">
    <h1 class="title">商品评价</h1>
    <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectD" :onlyContent="toggleD" :desc="desc" :ratings="ratings"></ratingselect>
    <div class="rating-wrapper">
      <ul v-show="ratings && ratings.length">
        <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in ratings" :key="rating.id" class="rating-item border-1px">
          <div class="user">
            <span class="name">{{ rating.username }}</span>
            <img class="avatar" width="12" height="12" :src="rating.avatar">
          </div>
          <div class="time">{{ rating.rateTime | formatDate }}</div>
          <p class="text">
            <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
          </p>
        </li>
      </ul>
      <div class="no-rating" v-show="!ratings || !ratings.length">暂无评价</div>
    </div>
  </div>
</template>

<script>
import {formatDate} from 'common/js/date';
import ratingselect from 'components/ratingselect/ratingselect';

const ALL = 2;

export default {
  data () {
    return {
      toggleD: this.onlyContent,
      selectD: this.selectType
    };
  },
  props: {
    ratings: {
      type: Array,
      default () {
        return [];
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: true
    },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        };
      }
    }
  },
  filters: {
    formatDate (time) {
      let date = new Date(time);
      return formatDate(date, 'yyyy-MM-dd hh:mm');
    }
  },
  methods: {
    needShow (type, text) {
      if (this.toggleD && !text) {
        return false;
      }
      if (this.selectD === ALL) {
        return true;
      } else {
        return type === this.selectD;
      }
    },
    selectRating (type) {
      this.selectD = type;
    },
    toggleContent () {
      this.toggleD = !this.toggleD;
    }
  },
  components: {
    ratingselect
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
.ratinglist
  padding-top: 18px
  .title
    line-height: 14px
    margin-left : 18px
    font-size: 14px
    color: rgb(7,17,27)
  .rating-wrapper
    padding: 0 18px
    .rating-item
      position: relative
      padding: 16px 0
      border-1px(rgba(7,17,27,0.1))
      .user
        position: absolute
        right: 0
        top: 16px
        line-height: 12px
        font-size: 0
        .name
          display: inline-block
          margin-right: 6px
          vertical-align: top
          font-size: 10px
          color: rgb(147,153,159)
        .avatar
          border-radius: 50%
      .time
        margin-bottom: 6px
        line-height: 12px
        font-size: 10px
        color: rgb(147,153,159)
      .text
        line-height: 16px
        font-size: 12px
        color: rgb(7,17,27)
        .icon-thumb_up, .icon-thumb_down
          margin-right: 4px
          line-height: 16px
          font-size: 12px
        .icon-thumb_up
          color: rgb(0,160,220)
        .icon-thumb_down
          color: rgb(147,153,159)
    .no-rating
      padding: 16px 0
      font-size: 12px
      color: rgb(147,153,159)
</style>
