<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodWrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">${{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">${{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

import BScroll from "better-scroll";

//常量
const ERROR_OK = 1;

export default {
  props:["seller"],
  data() {
    return {
      goods: [],
      classMap: [],
      menuScroll: ''
    };
  },
  created() {
    var that = this;
    that.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    that.$http.get("/api/goods").then(function(res){
      var json = res.data;
      if(json.code === ERROR_OK){
        that.goods = json.data;
        that.$nextTick(function(){
          that._initScroll();
        })
      }
    });
  },
  methods: {
    _initScroll: function(){
      var that = this;
      that.menuScroll = new BScroll(that.$refs.menuWrapper,{

      });
      that.foodsScroll = new BScroll(that.$refs.foodWrapper,{

      });
    }
  }
};

</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin";
  .goods
    display: flex;
    position: absolute;
    top: 174px;
    width: 100%;
    bottom: 46px;
    overflow: hidden;
    .menu-wrapper
      flex: 0 0 80px;
      width: 80px;
      background-color: #f3f5f7;
      .menu-item
        display: table;
        height: 54px;
        width: 56px;
        line-height: 14px;
        padding: 0 12px;
        .icon
          display: inline-block;
          width: 16px;
          height: 16px;
          vertical-align: top;
          margin-right: 2px;
          background-size: 16px 16px;
          background-repeat: no-repeat;
          &.decrease
            bg-image('decrease_3');
          &.discount
            bg-image('discount_3');
          &.guarantee
            bg-image('guarantee_3');
          &.invoice
            bg-image('invoice_3');
          &.special
            bg-image('special_3');
        .text
          display: table-cell;
          font-size: 12px;
          width: 56px;
          vertical-align: middle;
          border-1px(rgba(7, 17, 27, 0.1));
    .foods-wrapper
      flex: 1;
      .title
        padding-left: 14px;
        height: 26px;
        line-height: 26px;
        border-left: 2px solid #e9dde1;
        font-size: 12px;
        color: rgb(147, 153, 159);
        background-color: #f3f5f7;
      .food-item
        display: flex;
        margin: 18px;
        padding-bottom: 18px;
        border-1px(rgba(7, 17, 27, 0.1));
        &:last-child
          border-none();
          padding-bottom: 0;
        .icon
          flex: 0 0 57px;
          margin-right: 10px;
        .content
          flex: 1;
          .name
            margin: 2px 0 8px 0;
            height: 14px;
            font-size: 14px;
            line-height: 14px;
            color: rgb(7, 17, 27);
          .desc, .extra
            line-height: 10px;
            font-size: 10px;
            color: rgb(147, 153, 159);
          .desc
            margin-bottom: 8px;
            line-height: 12px;
          .extra
            .count
              margin-right: 5px;
          .price
            font-weight: 700;
            line-height: 24px;
            .now
              margin-right: 8px;
              font-size: 14px;
              color: rgb(240, 20, 20);
            .old
              text-decoration: line-through;
              font-size: 12px;
              color: rgb(147, 153, 159);
</style>