<template>
  <div class="backtop" @click="backtop"></div>
</template>

<script>
export default {
  name: "BackTop",
  setup(propa,{emit}){
    return{
      backtop:()=>{
        emit('bTop')
      }
    }
  }
}
</script>

<style scoped>
  .backtop{
    height: 40px;
    width: 40px;
    position: fixed;
    right: 30px;
    bottom: 80px;
    z-index: 30;
    box-shadow: 3px 3px 10px #888888;
    border-radius: 50%;
    background:#FCFCFC url("~assets/images/shang.png") center center;
  }
</style>