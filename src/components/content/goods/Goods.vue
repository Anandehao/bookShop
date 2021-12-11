<template>
  <div class="goods">
    <Goodsitem v-for="item in goods" :key="item" :product="item"></Goodsitem>
  </div>


</template>

<script>
import Goodsitem from "@/components/content/goods/Goodsitem";
export default {
name: "Goods",
  props:{
    goods:{
      type:Array,
      default(){
        return []
      }
    }
  },
  components:{
    Goodsitem
  }
}
</script>

<style scoped lang="scss">
  .goods{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    padding: 5px;
  }
</style>