<template>

  <div class="home">
    <nav-bar>
      <template v-slot:default>图书兄弟</template>
    </nav-bar>
    <tab-control v-show="isTabFixed" @tabclick="tabclick" :titles="['畅销','新书','精品']"></tab-control>

    <div class="wrapper">
      <div class="content">

        <div ref="banref">
          <home-swiper style="margin-top: 45px;" :banners="banners" ></home-swiper>
          <RecommendView :recommends="recommends"></RecommendView>
        </div>

        <tab-control @tabclick="tabclick" :titles="['畅销','新书','精品']"></tab-control>
        <Goods :goods="showGoods"></Goods>
      </div>
    </div>
    <back-top @bTop="bTop" v-show="isTabFixed"></back-top>
  </div>
</template>

<script>
import HomeSwiper from "@/views/home/ChildComps/HomeSwiper";
import NavBar from "@/components/common/navbar/NavBar";
import RecommendView from "@/views/home/ChildComps/RecommendView";
import TabControl from "@/components/content/tabcontrol/TabControl";
import Goods from "@/components/content/goods/Goods";
import BackTop from "@/components/common/backtop/BackTop";
import {getHomeAllData,getHomeGoods} from 'network/home';
import {ref,reactive,onMounted,computed,watchEffect,nextTick} from 'vue';
import BScroll from 'better-scroll';

export default {
  name: 'Home',

  setup(){
    let isTabFixed = ref(false);
    let banref = ref(null);
    const recommends=ref([]);
    const goods = reactive({
      sales:{page:1,list:[]},
      new:{page:1,list:[]},
      recommend:{page:1,list:[]}
    })
    let currentType = ref('sales');
    const showGoods = computed(()=>{
      return goods[currentType.value].list;
    })
    let bscroll = reactive({});
    let banners = ref([]);

    onMounted(()=>{
      getHomeAllData().then(res=>{
        //console.log(res);
        recommends.value = res.goods.data;
        banners.value=res.slides;
      })
      getHomeGoods('sales').then(res=>{
        //console.log(res);
        goods.sales.list = res.goods.data;
      })

      getHomeGoods('recommend').then(res=>{
        //console.log(res);
        goods.recommend.list = res.goods.data;
      })

      getHomeGoods('new').then(res=>{
       // console.log(res);
        goods.new.list = res.goods.data;
      })

      //创建BetterScroll对象
      bscroll = new BScroll(document.querySelector('.wrapper'),{
        probeType:3,
        click:true,  //是否允许点击
        pullUpLoad:true,
      });
      //触发滚动事件
      bscroll.on('scroll',(position)=>{
      //  console.log(-position.y);
      //  console.log(banref.value.offsetHeight);
        isTabFixed.value = (-position.y) > banref.value.offsetHeight;
      })

      //上拉加载数据
      bscroll.on("pullingUp",()=>{
      //  console.log("加载")
      //  console.log(document.querySelector('.center').clientHeight);

        const page = goods[currentType.value].page+1;

        getHomeGoods(currentType.value,page).then(res=>{
          goods[currentType.value].list.push(...res.goods.data);
          goods[currentType.value].page+=1;
        })
        //完成上拉，等待数据请求完成，要将新数据展示出来
         bscroll.finishPullUp();
         console.log("当前类型："+currentType.value+",当前页："+page);
        bscroll.refresh();

      })

    })
    const tabclick = (index)=>{
      let types = ['sales','new','recommend'];
      nextTick(()=>{
        bscroll && bscroll.refresh();
      })

    watchEffect(()=>{
      nextTick(()=>{
        bscroll && bscroll.refresh();
      })
    })
    }
    const bTop = () =>{
      bscroll.scrollTo(0,0,500);
    }

    return{
      recommends,
      tabclick,
      goods,
      showGoods,
      isTabFixed,
      banref,
      bTop,
      banners
    }
  },
  components: {
    HomeSwiper,
    NavBar,
    RecommendView,
    TabControl,
    BackTop,
    Goods
  }
}
</script>

<style scoped>
.banners img {
  width: 100%;
  height: 200px;
}
.home {
  height: 100vh;
  position: relative;
}
  .wrapper{
    position: absolute;
    top: 0px;
    bottom: 50px;
    left:0px;
    right: 0px;
    overflow: hidden;
    background: white;
  }
</style>
