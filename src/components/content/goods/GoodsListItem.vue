<template>
  <div class="good">
    <img :src="good.show.img" alt="">
    <div class="good-info">
      <p>{{good.title}}</p>
      <span class="price">{{good.price}}</span>
      <span class="collect">{{good.cfav}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "GoodsListItem",
    props: {
      good: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
  .good {
    padding-bottom: 40px;
    position: relative;
  }

  .good img {
    width: 100%;
    height: 240px;
    object-fit: cover;
    border-radius: 5px;
  }

  .good-info {
    font-size: 12px;
    text-align: center;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 6px;
  }

  .good-info p {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-bottom: 3px;
  }

  .good-info .price {
    color: var(--color-high-text);
    margin-right: 20px;
  }

  .good-info .collect {
    position: relative;
  }

  .good-info .collect::before {
    content: '';
    position: absolute;
    left: -15px;
    top: -1px;
    width: 14px;
    height: 14px;
    /*
    第一个0表示背景颜色 0代表透明色
    第二个0/14px
    0表示backgroud-position 设置背景图片的起始位置
    14px表示backgroud-size
    最后的14px与前面的14px共同构成backgroud-size(最后的14px可省略)
    */
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
  }
</style>
