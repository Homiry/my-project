<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    
      <tab-control class="tab-control" 
                  :titles="['流行','新款','精选']"  
                  @tabClick="tabClick" 
                  ref="tabControl1" 
                  v-show="isTabFixed"/>

    <scroll class="content" 
            ref="scroll" 
            :probe-type="3"
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
           
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view />
      <tab-control :titles="['流行','新款','精选']"  
                   @tabClick="tabClick" 
                   ref="tabControl2"/>
      <goods-list :goods="showGoods" />
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper';
  import RecommendView from './childComps/RecommendView';
  import FeatureView from './childComps/FeatureView';

  import NavBar from 'components/common/navbar/NavBar';
  import TabControl from 'components/content/tabControl/TabControl';
  import GoodsList from 'components/content/goods/GoodsList';
  import Scroll from 'components/common/scroll/Scroll';
  import BackTop from 'components/content/backTop/BackTop';


  import { getHomeMultidata, getHomeGoods } from "network/home";
  import { debounce } from "components/common/utils";

  export default {
    name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodsList,
      Scroll,
      BackTop
    },
     data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        currentType: 'pop',
        isShowBackTop: false,
        tabOffsetTop: 0,
        isTabFixed: false,
        saveY: 0
      }
    },
    
    activated() {
      this.$refs.scroll.scrollTo(0, this.saveY, 0)
      // 解决返回时不在记录的位置
      this.$refs.scroll.refresh()
    },

    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY()
    },

    created() {
      //先请求多个数据
      this.getHomeMultidata()

      //请求Goods数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    mounted() {
      //监听item中的图片加载完成
      const refresh = debounce(this.$refs.scroll.refresh, 50)
      this.$bus.$on('itemImageLoad', () =>{
          // console.log('--------')
          // this.$refs.scroll.refresh()
          refresh() 
        })
    },

    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    methods: {
      /*
       *事件监听方法
      */

      tabClick(index) {
        // console.log(index);
        switch(index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
        //使两个tabControl点击统一
        this.$refs.tabControl1.currentIndex = index;
        this.$refs.tabControl2.currentIndex = index;
      },
      backClick() {
        // 通过ref到Scroll组件中拿到对象的方法实现点击跳回顶部，奈斯
        this.$refs.scroll.scrollTo(0, 0, 500)
      },
      contentScroll(position) {
        // console.log(position)
        //判断BackTop（回到顶部箭头）是否显示
        this.isShowBackTop = (-position.y) > 1000
        //决定tabControl是否吸顶
        this.isTabFixed = (-position.y) > this.tabOffsetTop
      },
      loadMore() {
        // console.log('上拉加载')
        this.getHomeGoods(this.currentType)
      },
      swiperImageLoad() {
        //获取tabControl的offsetTop的值,组件没有offsetTop所以只能通过组件属性$el获取组件的元素offsetTop
        this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
      },
      /*
       * 网络请求方法
       */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
        // console.log(res)
      })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1 
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1;
          // console.log(res);
          //解决第二次上拉加载上不生效（better scroll默认下拉加载一次。）
          this.$refs.scroll.finishPullUp()
      })
      }
    }
  }
</script>

<style scoped>
  #home {
    position: relative;
    /* padding-top: 44px; */
    height: 100vh;
  }

  .home-nav {
    /* position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
    background-color: var(--color-tint);
    color: #fff;
  }

  .tab-control {
    position: relative;
    z-index: 9;
  }

  .content {
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
    overflow: hidden;
  }

  

  /* .content {
    height: calc(100% - 93px);
    overflow: hidden;
    margin-top: 44px;
  } */

</style>
