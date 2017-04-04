<template>
  <div class="app">
    <!-- 头部信息 -->
    <v-header :seller="seller"></v-header>
    <!-- tabs区域 -->
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>

<script>

import topHeader from '@/components/header/header';
//常量
const ERROR_OK = 1;

export default {
  data() {
    return {
      seller: {}
    }
  },
  created() {
    var that = this;
    that.$http.get("/api/seller").then(function(res){
      var json = res.data;
      if(json.code === ERROR_OK){
        that.seller = json.data;
      }
    });
  },
  components:{
    'v-header': topHeader
  }
};

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin";
  .tab
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex:1 ;
      text-align: center;
      & > a 
        display: block;
        font-size: 14px;
        color: rgb(77, 85, 93);
        &.active
          color: rgb(240, 20, 20);
</style>
