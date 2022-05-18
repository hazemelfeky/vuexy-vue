<template>
  <div class='product--row comp'>
    <div class='product--row__left'>
      <img alt='product' :src="product.img" />
    </div>
    <div class='product--row__center'>
      <h3 class='product--row__center__name'>{{product.name}}</h3>
      <h5 class='product--row__center__company'>by <span>{{product.brand}}</span></h5>
      <div class='product--row__center__stars'>
        <ul>
          <li v-for="i in 5" :key="i">
            <StarIcon v-if="i<=product.rating" :fill="true" />
            <StarIcon v-else :stroke="true" />
          </li>
        </ul>
      </div>
      <p class='product--row__center__description'>{{product.discription}}</p>
    </div>
    <div class='product--row__right'>
      <div class='product--row__right__price'>${{product.price}}</div>
      <div class='product--row__right__up'>
        <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-50 feather feather-heart"><path data-v-15eba8c6="" d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path></svg>
        <h4>Wishlist</h4>
      </div>
      <div @click="handleAddToCart()" class='product--row__right__down'>
        <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-50 feather feather-shopping-cart"><circle data-v-15eba8c6="" cx="9" cy="21" r="1"></circle><circle data-v-15eba8c6="" cx="20" cy="21" r="1"></circle><path data-v-15eba8c6="" d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>
        <h4>Add In Cart</h4>
      </div>
    </div>
  </div>
</template>

<script>
import StarIcon from './StarIcon.vue'

export default {
  name:"ProductRow",
  props: ['product', 'add-to-cart'],
  components: { StarIcon },
  methods: {
    handleAddToCart(){
      this.$parent.$emit('add-to-cart', this.product)
    }
  }
}
</script>

<style lang="scss">
@import '../../styles/varibles.scss';

.product--row{
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  width: 100%;
  border-radius: $border-radius;
  box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
  overflow: hidden;
  height: fit-content;

  &:hover{
    transform: translateY(-5px);
    box-shadow: 0 4px 24px 0 RGB(34 41 47 / 25%);
  }
  &__left{
    height: 100%;
    padding: 0;
    display: flex;
    align-items: center;
    img{
      width: 100%;
    }
  }
  &__center{
    padding: 1.5rem 1rem;
    display: flex;
    flex-flow: column;
    gap: 0.5rem;
    &__name{
      font-size: 14px;
      font-weight: 600;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-overflow: ellipsis;
      transition: all 0.5s ease;
      color: $text--light;
      &:hover{
        color: $icons;
        cursor: pointer;
      }
    }
    &__company{
      text-transform: capitalize;
      font-size: 12.25px;
      font-weight: normal;
      span{
        color: $icons;
        font-weight: bold;
      }
    }
    &stars{
      display: flex;
    }
    &__description{
      font-size: 12.25px;
      line-height: 1.5rem;
      display: -webkit-box;
      -webkit-line-clamp: 5;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }
  &__right{
    padding: 1rem;
    display: flex;
    gap: 1rem;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border-left: 1px solid #ccc;
    &__price{
      font-size: 1.286rem;
      color: $icons;
      font-weight: 500;
    }
    &>div{
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 0.5rem;
      width: 100%;
      border-radius: 5px;
      cursor: pointer;
      h4{
        font-weight: 600;
      }
    }
    &__up{
      padding: 0.786rem 1.5rem;
      background-color: #f6f6f6;
      &:hover{
        background-color: #e3e3e3;
      }
      h4{
        color: $text--light;
      }
    }
    &__down{
      color: #fff;
      background-color: $icons;
      padding: 0.786rem 0;
      transition: all 0.2s ease;
      &:hover{
        box-shadow: 0 8px 25px -8px #7367f0;
      }
    }
  }
  @include breakpoint(sm){
    display: flex;
    flex-direction: column;

    &__left {
      padding: 1rem 0;
      justify-content: center;
      img {
        width: auto;
      }
    }
    &__right{
      border: none;
    }
  }
}
</style>