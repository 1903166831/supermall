<template>
  <div class="recommend">
    <div v-for="(item, index) in recommends" :key="index" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <p>{{item.title}}</p>
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: "RecommendView",
    props: {
      recommends: {
        type: Array,
        default() {
          return []
        }
      }
    }
  }
</script>

<style scoped>
  .recommend{
    display: flex;
    padding: 10px 0 20px;
    border-bottom: 10px solid #eee;
  }

  .recommend-item{
    flex: 1;
    text-align: center;
    font-size: 14px;
  }

  .recommend-item img{
    width:75%;
    margin-bottom: 10px;
  }
</style>
