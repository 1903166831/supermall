<template>
  <div class="back">
    <img src="~assets/img/common/top.png" alt="">
  </div>
</template>

<script>
  export default {
    name: "ScrollTop",

  }
</script>

<style scoped>
  .back {
    position: fixed;
    bottom: 50px;
    right: 8px;
  }

  .back img{
    width: 43px;
    height: 43px;
  }
</style>
