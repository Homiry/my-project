<template>
  <div class="goods">
    <goods-list-item 
      class="item"
      v-for="(item,index) in goods" 
      :goods-item="item" 
      :key="index"/>
  </div>
</template>

<script>
  import GoodsListItem from './GoodsListItem'

  export default {
    name: "GoodsList",
    components: {
      GoodsListItem
    },
    props: {
      goods: {
        type: Array,
        default() {
          return []
        }
      }
    }
  }
</script>

<style scoped>
  .goods {
    display: flex;
    flex-wrap: wrap;
    padding: 5px;
    justify-content: space-around;
  }

  .goods .item {
      width: 48%;
  }
</style>