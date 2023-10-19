<script>
import bg from './components/bg.vue'
import fish from './views/fish.vue'
import test from './views/test.vue'
import test2 from './views/test2.vue'
export default{
    data(){
        return{

        }
    },
    methods:{

    },
    components:{
      bg,
      fish,
      test,
      test2,

    }
}
</script>

<template>
<div class="all">

  <div class="header">
    <i class="fa-solid fa-sun"></i>
  </div>
  <div class="contain">
    <bg />
    <fish />
  </div>
  <div class="footer">
    
  </div>
</div>
<div class="test">
  <!-- <test /> -->
  <!-- <test2 /> -->
</div>
</template>

<style lang="scss" scoped>
.all{
  width: 99vw;
  height: 96vh;
  .header{
    width: 100%;
    height: 20%;
    border: 1px solid;
    position: relative;
    .fa-sun{
      position: absolute;
      right: 50px;
      top: 50px;
      font-size: 80pt;
    }
  }
  .contain{
    width: 100%;
    height: 60%;
    border: 1px solid;

  }
  .footer{
    width: 100%;
    height: 20%;
    border: 1px solid;
    background-color: black;
  }
}
</style>
