<template>
  <div id="tab-bar">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'TabBar'
}
</script>

<style scoped>
  #tab-bar {
    height: 49px;
    display: flex;
    background: #f5f5f5;
    box-shadow: 0 -2px 2px rgba(0, 0, 0, .1);
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
  }
</style>
