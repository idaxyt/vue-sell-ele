<template>
  <div id="app">
    <v-header :seller='seller'></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :goods='goods' :seller='seller' :ratings='ratings'></router-view>
    </keep-alive>
  </div>
</template>

<script>
import VHeader from './components/v-header/v-header'
import { urlParse } from './utils/index'
//OK状态码
const ERR_OK = 0
export default {
  name: "app",
  data() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          return queryParam.id
        })()
      },
      goods: {},
      ratings: []
    }
  },
  components: {
    VHeader
  },
  created() {
    this.$http.get('/api/seller?id='+this.seller.id).then((res) => {
      res = res.body
      if(res.errno===ERR_OK) {
          this.seller = Object.assign({},this.seller,res.data)
      }
    }),
    this.$http.get('api/goods').then((res) => {
      res = res.body
      if(res.errno == ERR_OK) {
        this.goods = res.data
      }
    }),
    this.$http.get('api/ratings').then((res) => {
      res = res.body
      if(res.errno == ERR_OK) {
        this.ratings = res.data
      }
    })
  }
};
</script>

<style lang='stylus'>
@import './common/stylus/mixin.styl'
#app 
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  .tab
    display: flex
    width:  100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      text-align: center
      flex: 1
      a
        display: block
        text-decoration: none 
        font-size: 14px
        color: rgb(77,85,93)
      .active
        color: rgb(240,20,20)
</style>
