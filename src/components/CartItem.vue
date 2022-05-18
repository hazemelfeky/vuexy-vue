<template>
  <div class='item'>
    <img :src="product.img" alt='cart item' />
    <div class='item__details'>
      <div class='item__details__name'>
        <div class='item__details__name__title'>{{product.name}}</div>
        <div class='item__details__name__brand'>By {{product.brand}}</div>
      </div>
      <div class='item__details__control'>
        <span @click="handleDecrease(product.id)">-</span>
        <span>{{product.count}}</span>
        <span @click="handleIncrease(product.id)">+</span>
      </div>
      <div class='item__details__price'>${{product.price}}</div>
    </div>
    <div class='item__close' @click="() => handleDeleteFromCart(product)">X</div>
  </div>
</template>

<script>
export default {
  name:"CartItem",
  props: [ 'product' ],
  data(){
    return {
      count: 0
    }
  },
  methods: {
    handleDeleteFromCart(prod) {
      this.$parent.$emit('delete-from-cart', prod)
    },
    handleIncrease(id) {
      this.$parent.$emit('increase-product', id)
    },
    handleDecrease(id) {
      this.$parent.$emit('decrease-product', id)
    },
  },
}
</script>

<style lang="scss">
  @import '../styles/varibles.scss';

  .item{
    display: flex;
    padding: 1rem;
    gap: 1rem;
    position: relative;

    img{
      background-color: #f8f8f8;
      border-radius: 6px;
      width: 62px;
    }
    &__details{
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: -webkit-fill-available;

      &__name{
        width: 8rem;
        display: flex;
        gap: 0.4rem;
        flex-direction: column;

        &__title{
          display: -webkit-box;
          -webkit-line-clamp: 2;
          -webkit-box-orient: vertical;
          overflow: hidden;
          font-weight: 500;
          line-height: 1.2;
          color: #5e5873;
        }
        &__brand{
          color: #b9b9c3;
          font-size: 12px;
        }
      }

      &__control{
        display: flex;
        border-radius: 6px;
        border: 1px solid #d8d6de;
        span{
          padding: 0.25rem 0.7rem;
          cursor: pointer;
          // border-bottom: 1px solid #ebe9f1;
          width: 30px;
          text-align: center;
        }
        span:not(:first-child){
          border-left: 1px solid #d8d6de;
        }
      }
      &__price{
        font-weight: 500;
      }
    }

    &__close{
      position: absolute;
      top: 1rem;
      right: 1rem;
      cursor: pointer;
      display: none;
    }
    &:hover &__close{
      display: block;
    }
  }
</style>