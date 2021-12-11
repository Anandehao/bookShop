<template>
  <div>
    <nav-bar>
      <template v-slot:default>商品分类</template>
    </nav-bar>

    <div class="mainbox">
      <div class="ordertab">
        <van-tabs v-model="active" @click="t" >
          <van-tab title="销量排序"></van-tab>
          <van-tab title="价格排序"></van-tab>
          <van-tab title="评论排序"></van-tab>
        </van-tabs>
      </div>
      <van-sidebar class="leftmenu" v-model="activeKey">
        <van-collapse v-model="activeName" accordion>
          <van-collapse-item v-for="item in categories" :key="item.id"
                              :title="item.name" :name="item.name">
            <van-sidebar-item v-for="(sub,index) in item.children"
                              :key="index"
                              :title="sub.name"
                              @click="getGoods(sub.id)"
            />
          </van-collapse-item>
        </van-collapse>
      </van-sidebar>


      <div class="goodslist">
        <div class="content">
<!--          <van-card-->
<!--            v-for="item in showGoods" :key="item.id"-->
<!--            :num="item.comments_count"-->
<!--            :tag="item.comment_count >=0 ? '流行' : '标签' "-->
<!--            :price="item.price"-->
<!--            :desc="item.updated_at"-->
<!--            :title="item.title"-->
<!--            :thumb="item.cover_url"-->
<!--            :lazy-load="true"-->
<!--          />-->

        </div>
      </div>

    </div>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar";

import {ref, reactive,computed, onMounted} from 'vue';
import {getCategory,getCategoryGoods} from "@/network/category";

export default {
name: "Category",
  setup(){
    let active = ref(1);
    let activeKey = ref(0);
    let categories = ref([]);
    let activeName = ref(1);

    //当前的排序条件
    let currentOrder = ref('sales')
    //当前的分类ID
    let currentCid = ref(0)
    //数据模型
    // const goods = reactive({
    //   sales:{page:1,list:[]},
    //   price:{page:1,list:[]},
    //   comments_count:{page:1,list:[]},
    // })

    // const showGoods = computed(()=>{
    //   return goods[currentOrder.value].list
    // })

    // const init = () =>{
    //   getCategoryGoods('sales',currentCid.value).then(res=>{
    //     goods.sales.list = res.goods.list
    //   })
    //   getCategoryGoods('price',currentCid.value).then(res=>{
    //     goods.price.list = res.goods.list
    //   })
    //   getCategoryGoods('comments_count',currentCid.value).then(res=>{
    //     goods.comments_count.list = res.goods.list
    //   })
    // }
    onMounted(()=>{
      getCategory().then(res=>{
        categories.value = res.categories
        console.log(res);
      })

    //  init()
    })

    // const tabClick = (index)=>{
    //   let orders = ['sales','price','comments_count']
    //   currentOrder.value = orders[index]
    //   // getCategoryGoods(currentOrder.value,currentCid.value).then(res=>{
    //   //   goods[currentOrder.value].list = res.goods.list
    //   // })
    //
    //   console.log("排序的序号:"+currentOrder.value)
    //   console.log("当前分类id:"+currentCid.value)
    //
    // }
    const getGoods = (cid) =>{
      currentCid.value = cid
    //  init()
      console.log("排序的序号:"+currentOrder.value)
      console.log("当前分类id:"+currentCid.value)

    }
    return{
      activeKey,
      categories,
      activeName,
      active,
   //   tabClick,
      currentOrder,
      currentCid,
      getGoods,
   //   showGoods
    }
  },
  components: {
    NavBar
  }
}
</script>

<style scoped lang="scss">
.mainbox{
  display: flex;
  .ordertab{
    height: 50px;
    position: fixed;
    top: 45px;
    left: 130px;
    right: 0px;
  }
  .leftmenu{
    width: 130px;
    position: fixed;
    top: 95px;
    left: 0;
    color: gray;
  }
  .goodslist{

    height: 100vh;
    position: absolute;
    top: 100px;
    left: 130px;
    right: 0;
    padding: 10px;
  }
}
</style>