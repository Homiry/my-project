<template>
  <div id="detail">
    <detail-nav-bar/>
    <detail-swiper :top-images="topImages"/>
    <detail-base-info :goods="goods"/>
    <detail-shop-info :shop="shop"/>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo'
  import DetailShopInfo from './childComps/DetailShopInfo'

  import { getDetail, Goods,Shop } from 'network/detail'

  export default {
    name: "Detail",
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo
    },
    data() {
      return {
        iid: null,
        topImages: [],
        // goods: null
        goods: {},
        shop: {}
      }
    },

    created() {
      //保存传入的iid
      this.iid = this.$route.params.iid

      //根据iid请求详情数据
      getDetail(this.iid).then(res => {
        console.log(res);
        const data = res.result;
        //获取轮播图
        this.topImages = data.itemInfo.topImages;
        //获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        //创建店铺信息的对象
        this.shop = new Shop(data.shopInfo)
      })
    }
  }
</script>

<style scoped>

</style>