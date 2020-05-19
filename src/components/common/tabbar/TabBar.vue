<template>
  <div id="tab-bar">
    <slot></slot>
  </div>
</template>

<script>
  export default {
    name: "TabBar",
    data() {
      return {
        homeImg: '../../ass'
      }
    }
  }
</script>

<style scoped>
  #tab-bar {
    display: flex;
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;

    /* x轴正值向右 y轴正值向下 */
    box-shadow: 0 -1px 1px rgba(100, 100, 100, .2);
    background-color: #fff;
  }
</style>
