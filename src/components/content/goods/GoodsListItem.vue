<template>
  <div class="goods-list-item">
    <img :src="goodsitem.show.img" alt="">
    <div>
      <p>{{goodsitem.title}}</p>
      <span class="price">{{goodsitem.price}}</span>
      <span class="collect">{{goodsitem.cfav}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "GoodsListItem",
    props: {
      goodsitem: {
        type: Object,
        default() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
  .goods-list-item{
    text-align: center;
    margin-bottom: 2%;
    font-size: 14px;
  }

  .goods-list-item img{
    width: 100%;
    border-radius: 6px;
  }

  .goods-list-item p{
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    margin-top: 3px;
    font-size: 16px;
  }

  .goods-list-item .price{
    color: var(--color-high-text);
  }

  .goods-list-item .price::after{
    content: '';
    display: inline-block;
    width: 14px;
    height: 14px;
    margin: 0 0 0 3px;
    vertical-align: top;
    background: url('~assets/img/common/collect.svg') no-repeat center;
    background-size: 100%;
  }
</style>
