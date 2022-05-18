<template>
  <div class="app" :class="{ dark: dark }">
    <div @click="handleOverlay" class="overlay" :class="{ 'show': showDrawer}" ></div>
    <Drawer :closeDrawer="closeDrawer" :showDrawer="showDrawer" />
    <div class="app__main--content container">
      <Navbar @increase-product="increaseProduct" @decrease-product="decreaseProduct" @delete-from-cart="handleDeleteFromCart" :cartProducts="cartProducts" :isDark="dark" :useDrawer="useDrawer" :useDark="useDark"/>
      <div class="app__main--content__down">
        <SecondNavbar />
        <Products @add-to-cart="handleAddToCart" @handleShowFilters="useFilters" :showingFilters="showFilters" />
      </div>
    </div>
    <ScrollButton />
  </div>
</template>

<script>
import { ref } from 'vue'
import Drawer from './Drawer.vue'
import Navbar from './Navbar.vue'
import SecondNavbar from './SecondNavbar.vue'
import Products from './Products.vue'
import ScrollButton from './ScrollButton.vue'

export default {
  name:'Layout',
  components: {
    Drawer,
    Navbar,
    SecondNavbar,
    Products,
    ScrollButton
  },
  setup() {
    const dark = ref(false)
    const showDrawer = ref(false)
    const showFilters = ref(false)
    const cartProducts = ref([])


    const handleOverlay = () => {
      showDrawer.value = false
      showFilters.value = false
    }

    const closeDrawer = () => {
      showDrawer.value = false
    }

    const useDrawer = () => {
      showDrawer.value = !showDrawer.value
    }
    
    const useFilters = () => {
      showFilters.value = !showFilters.value
      console.log('useFilters');
    }

    const useDark = () => {
      dark.value = !dark.value
    }

    const handleAddToCart = (prod) => {
      const isAlreadyInCart = cartProducts.value.filter( x => x.id === prod.id).length > 0
      if (!isAlreadyInCart) {
        cartProducts.value.push({...prod, count: 1})
      }
    }

    const increaseProduct = (id) => {
      cartProducts.value.filter( x => x.id == id)[0].count++
    }

    const decreaseProduct = (id) => {
      cartProducts.value.filter( x => x.id == id)[0].count == 1?
      handleDeleteFromCart(cartProducts.value.filter( x => x.id == id)[0]) 
      : cartProducts.value.filter( x => x.id == id)[0].count--
    }

    const handleDeleteFromCart = (prod) => {
      cartProducts.value = cartProducts.value.filter( (x) => x.id != prod.id )
    }

    return { dark, showDrawer, showFilters, cartProducts, increaseProduct, decreaseProduct, handleAddToCart, handleDeleteFromCart, handleOverlay, useFilters, closeDrawer, useDrawer, useDark }
  }
}
</script>

<style lang='scss'>
  @import '../styles/varibles.scss';

  .app{
    // height: 100vh;
    background-color: $container--light;
    display: flex;
    //light
    color: $text--light;

    .container{
      background-color: $container--light;
    }
    .overlay{
      position: fixed;
      top: 0;
      height: 120vh;
      width: 100%;
      z-index: 400;
      display: block;
      visibility: hidden;
      opacity: 0;
      background-color: rgba(34,41,47,.5);
      transition: all .5s ease;
      &.show{
        opacity: 1;
        visibility: visible;
      }
    }
    .comp{
      background-color: $comp--light;
    }
    &__main--content{
      width: -webkit-fill-available;
      margin-left: 260px;
      @include breakpoint(lg){
        margin-left: 0;
      }
      &__down{
        padding: 0 2rem;
        @include breakpoint(sm){
          padding: 0 1rem;
        }
      }
    }
    input[type='radio']:after{
      background-color: $comp--light;
    }

    &.dark{
      
      //dark
      color: $text--dark;
      .container{
        background-color: $container--dark;
      }
      .comp{
        background-color: $comp--dark;
        box-shadow: none;
      }
      a,h2,h4,p{
        color: $text--dark;
      }

      ///////
      .navbar{
        // box-shadow: 0 4px 24px 0 rgb(21 22 23 / 51%);
      }
      .drawer--group .active{
        background-color: $container--dark !important; 
      }
      .drawer--group h3{
        color: #676d7d;
      }
      input[type='radio']:after{
        background-color: $comp--dark;
      }
      .product__btns__up{
        background-color: #161d31;
        color: #fff;
      }
      .selector__choices{
        background-color: $comp--dark;
        color: #b4b7bd;
      }
      .slider > .thumb{
        background-color: $container--dark;
      }
      .product__details__name, .product__details__numbers__price{
        color: #b4b7bd;
      }
      .cart__title > h2{
        color: #d0d2d6;
      }
      .cart > div{
        border-color: #3b4253;
      }
      .item img{
        background-color: #161d31;
      }
      .item__details__control{
        border-color: #404656;
      }
      .item__details__control > span{
        border-color: #404656;
      }
      .item__details__name__title{
        color: #b4b7bd;
      }
      .item__details__control span{
        border-color: #3b4253;
      }
      .product__btns > div h4{
        color: #fff;
      }
      .products__right__pagination__list{
        background-color: #242b3d;
      }
      .products__right__pagination button{
        background-color: #242b3d;
      }
      .products__right__pagination button{
        &:disabled{
          opacity: 0.7;
        }
      }
      .product--row__right__up{
        background-color: #161d31;
      }
      .product--row__right{
        border-color: #3b4253;
      }
      .products__right__search input{
        color: $text--dark;
      }
      .product--row__center__name{
        color: $text--dark;

      }
    }

  }
</style>