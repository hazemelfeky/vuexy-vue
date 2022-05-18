<template>
  <div class='product comp'>
    <div class='product__image'>
      <img alt='product' :src="product.img" />
    </div>
    <div class='product__details'>
      <div class='product__details__numbers'>
        <div class='product__details__numbers__stars'>
          <!-- <StarIcon v-for="i in 5" :key="i" stroke /> -->
          <ul>
            <li v-for="i in 5" :key="i">
              <StarIcon v-if="i<=product.rating" :fill="true" />
              <StarIcon v-else :stroke="true" />
            </li>
          </ul>
        </div>
        <div class='product__details__numbers__price'>
          ${{product.price}} 
        </div>
      </div>
      <h3 class='product__details__name'>{{product.name}}</h3>
      <p class='product__details__discription'>{{product.discription}}</p>
    </div>
    <div class='product__btns'>
      <div class='product__btns__up'>
        <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-50 feather feather-heart"><path data-v-15eba8c6="" d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path></svg>
        <h4>Wishlist</h4>
      </div>
      <div @click="handleAddToCart()" className='product__btns__down'>
        <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mr-50 feather feather-shopping-cart"><circle data-v-15eba8c6="" cx="9" cy="21" r="1"></circle><circle data-v-15eba8c6="" cx="20" cy="21" r="1"></circle><path data-v-15eba8c6="" d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>
        <h4>Add In Cart</h4>
      </div>
    </div>
  </div>
</template>

<script>
import StarIcon from './StarIcon.vue'
export default {
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

.product{
  display: flex;
  flex-direction: column;
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
  &__image{
    min-height: 15.85rem;
    display: flex;
    align-items: center;
    img{
      width: 100%;
    }
  }
  &__details{
    padding: 1rem;
    width: 100%;
    color: #6e6b7b;
    &__numbers{
      display: flex;
      justify-content: space-between;

      &__price{
        font-weight: 600;
      }
    }
    &__name{
      font-weight: 600;
      font-size: 14px;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-overflow: ellipsis;
      margin-top: 0.75rem;
      transition: all 0.5s ease;
      &:hover{
        color: $icons;
        cursor: pointer;
      }
    }

    &__discription{
      font-size: .875rem;
      margin-top: 0.4rem;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }

  &__btns{
    display: flex;
    flex-direction: row;

    @include breakpoint(xlg){
      flex-direction: column;
    }

    @include breakpoint(md){
      flex-direction: row;
    }

    @include breakpoint(sm){
      flex-direction: column;
    }

    >div{
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 0.5rem;
      width: 50%;
      h4{
        font-weight: 600;
      }
      cursor: pointer;

      @include breakpoint(xlg){
        width: 100%;
      }
    }
    &__up{
      color: #2a2e30;
      background-color: #f6f6f6;
      padding: 0.786rem 0;
      
    }
    &__down{
      color: #fff;
      background-color: $icons;
      padding: 0.786rem 0;
    }
  }
}
</style>