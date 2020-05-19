<template>
  <div class="tab-control">
    <div v-for="(item,index) in titles" :key="index" @click="itemClick(index)">
      <span :class="{active: index === currentIndex}">{{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "tabControl",
    props: {
      titles: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {
        currentIndex: 0
      }
    },
    methods: {
      itemClick(index) {
        this.currentIndex = index
      }
    }
  }
</script>

<style scoped>
  .tab-control {
    display: flex;
    width: 100%;
    text-align: center;
    font-size: 15px;
    height: 40px;
    line-height: 40px;
    background-color: #fff;
  }

  .tab-control div {
    flex: 1;
  }

  .tab-control span {
    padding: 5px;
  }

  .active {
    color: var(--color-high-text);
    border-bottom: 3px solid var(--color-tint);
  }
</style>
