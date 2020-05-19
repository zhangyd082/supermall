<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <home-recommend-view :recommends="recommends"></home-recommend-view>
    <home-feature-view></home-feature-view>
    <tab-control :titles="['流行','新款','精选']"></tab-control>
    <goods-list :goods="goods['pop'].list"></goods-list>
  </div>
</template>

<script>
  import HomeSwiper from "./childComps/HomeSwiper";
  import HomeRecommendView from "./childComps/HomeRecommendView";
  import HomeFeatureView from "./childComps/HomeFeatureView";

  import NavBar from "components/common/navbar/NavBar";
  import TabControl from "components/content/tabControl/TabControl";
  import GoodsList from "components/content/goods/GoodsList";

  import {getHomeMultidata, getHomeGoods} from 'network/home'

  export default {
    name: "Home",
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},

        }
      }
    },
    components: {
      GoodsList,
      HomeSwiper,
      NavBar,
      HomeRecommendView,
      HomeFeatureView,
      TabControl
    },
    created() {
      // 1.请求多个数据
      this.getHomeMultidata()

      // 2.请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    methods: {
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          // 函数调用 -> 压入函数栈(保存函数调用过程中所有变量)
          // 函数调用结束 -> 弹出函数栈(释放函数所有变量)
          // 因此箭头函数执行完毕，res将没有任何意义，所以要在函数执行完毕之前将数据保存起来
          // this.result = res
          // 上面这行代码就相当于将内存地址赋给result
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          // push()原是传单个数据的，想传整个数组怎么办
          // 参数为数组，前加...可以将数组解析成一个个单独的数据加进去
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
        })
      }
    }
  }
</script>

<style scoped>
  .home-nav {
    background: var(--color-tint);
    color: #fff;
  }
</style>
