<template>
  <div class="tab-control">
    <div class="tab-control-item" v-for="(item, index) in titles" :key="index" :class="{active: index === currentIndex}" @click="btnClick(index)">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    data() {
      return {
        currentIndex: 0
      }
    },
    props: {
      titles: {
        type: Array,
        default() {
          return []
        }
      }
    },
    methods: {
      btnClick(index) {
        this.currentIndex = index
        this.$emit("tabClick", index)
      }
    }
  }
</script>

<style scoped>
  .tab-control{
    display: flex;
    text-align: center;
    height: 40px;
    line-height: 40px;
    font-size: 14px;
    background: #fff;
  }

  .tab-control-item{
    flex: 1;
  }

  .active{
    color: var(--color-high-text);
  }

  .active span{
    padding: 5px;
    border-bottom: 2px solid var(--color-high-text);
  }
</style>
