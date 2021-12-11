<template>
  <div class="tab-control">
    <div v-for="(item,index) in titles" :key="index" :class="{active:index == currentIndex}" @click="itemclick(index)" class="tab-control-item">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
name: "TabControl",
props:{
  titles:{
    type:Array,
    default() {
      return [];
    }
  }
},
setup(props,{emit}){
  let currentIndex = ref(0);
  const itemclick = (index)=>{
    currentIndex.value=index;
    emit('tabclick',index)
  }
  return{
    currentIndex,
    itemclick
  }
}
}
</script>

<style scoped lang="scss">
.tab-control{
  display: flex;
  height: 40px;
  line-height: 40px;
  text-align: center;
  background-color: white;
  width: 100%;
  position: sticky;
  top: 45px;
  z-index: 10;
}
.tab-control-item{
  flex: 1;
  span{
    padding: 6px;
  }
}
.active{
  color: var(--color-tint);
  span{
    border-bottom: 3px solid var(--color-tint);
  }
}
</style>