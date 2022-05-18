<template>
  <div class='navbar comp'>
    <div class='navbar__left'>
      <a href='#' @click="useDrawer" class='burger--menu'><svg xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-menu"><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"></line><line x1="3" y1="18" x2="21" y2="18"></line></svg></a>
      <a href='#'><svg data-v-1134b199="" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect data-v-1134b199="" x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line data-v-1134b199="" x1="16" y1="2" x2="16" y2="6"></line><line data-v-1134b199="" x1="8" y1="2" x2="8" y2="6"></line><line data-v-1134b199="" x1="3" y1="10" x2="21" y2="10"></line></svg></a>
      <a href='#'><svg data-v-1134b199="" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-message-square"><path data-v-1134b199="" d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z"></path></svg></a>
      <a href='#'><svg data-v-1134b199="" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-mail"><path data-v-1134b199="" d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"></path><polyline data-v-1134b199="" points="22,6 12,13 2,6"></polyline></svg></a>
      <a href='#'><svg data-v-1134b199="" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-check-square"><polyline data-v-1134b199="" points="9 11 12 14 22 4"></polyline><path data-v-1134b199="" d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"></path></svg></a>
    </div>
    <div class='navbar__right'>
      <a href='#' @click="useDark" >
        <svg v-if="!isDark" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-moon"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path></svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-sun"><circle cx="12" cy="12" r="5"></circle><line x1="12" y1="1" x2="12" y2="3"></line><line x1="12" y1="21" x2="12" y2="23"></line><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line><line x1="1" y1="12" x2="3" y2="12"></line><line x1="21" y1="12" x2="23" y2="12"></line><line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line><line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line></svg>
      </a>
      <a href='#'><svg data-v-0e8a7f4f="" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-search"><circle data-v-0e8a7f4f="" cx="11" cy="11" r="8"></circle><line data-v-0e8a7f4f="" x1="21" y1="21" x2="16.65" y2="16.65"></line></svg></a>
      <a href='#' @click="handleShowCart">
        <svg data-v-a58fee00="" xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-body feather feather-shopping-cart"><circle data-v-a58fee00="" cx="9" cy="21" r="1"></circle><circle data-v-a58fee00="" cx="20" cy="21" r="1"></circle><path data-v-a58fee00="" d="M1 1h4l2.68 13.39a2 2 0 0 0 2 1.61h9.72a2 2 0 0 0 2-1.61L23 6H6"></path></svg>
        <div class='badge' v-if="cartProducts.length">{{cartProducts.length}}</div>
      </a>
      <a href='#'><svg xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-body feather feather-bell"><path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"></path><path d="M13.73 21a2 2 0 0 1-3.46 0"></path></svg></a>
      <a href='#' class='navbar__right__profile'>
        <div class='navbar__right__profile__name'>
          <h4>John Doe</h4>
          <p>admin</p>
        </div>
        <div class='navbar__right__profile__photo'>
          <img src='/pizza.jpg' />
          <span class='online'></span>
        </div>
      </a>
    </div>
    <div class="cart comp " :class="{show: showCart}" >
      <div class='cart__title'>
        <h2>My Cart</h2>
        <div class='cart__title__count'>{{cartProducts.length}} items</div>
      </div>

      <div v-if="cartProducts.length">
        <div class='cart__items'>
          <CartItem v-for="prod in cartProducts" :key="prod.id" :handleDeleteFromCart='handleDeleteFromCart' :product="prod" />
        </div>
        <div class='cart__total'>
          <div class='cart__total__top'>
            <h4>Total</h4>
            <h4 class='cart__total__top__money'>${{Math.round(cartProducts.reduce((a, b) => a + (b.price * b.count), 0))}}</h4>
          </div>
          <div class='cart__total__bot'>
            Checkout
          </div>
        </div>
      </div> 
      <div class='cart__empty' v-else>Your cart is empty</div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import CartItem from './CartItem.vue'

export default {
  name:'Navbar',
  components: { CartItem },
  props: [ 'useDrawer', 'useDark', 'isDark', 'cartProducts', 'handleDeleteFromCart' ] ,
  setup() {
    const showCart = ref(false)
    const handleShowCart = () => {
      // !showCart && setShowCart(true)
      // if (!showCart) {
      //   showCart.value = true
      // }
      showCart.value = !showCart.value
    }

    return { handleShowCart, showCart }
  }
}
</script>

<style lang="scss">
  @import '../styles/varibles.scss';

.navbar{
  border-radius: $border-radius;
  display: flex;
  justify-content: space-between;
  margin: 1.3rem 2rem 0;
  padding: 0.8rem 1rem;
  box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
  position: sticky;
  top: 1.3rem;
  z-index: 50;

  >div{
    
    a{
      position: relative;
      display: flex;
      font-size: 22px;
      padding: 0 7px;
    }
  }

  &__left{
    display: flex;
    align-items: center;

    a:not(:first-of-type){
      @include breakpoint(sm){
        display: none;
      }
    }
  }

  &__right{
    display: flex;
    align-items: center;
    &__profile{
      display: flex;

      &__name{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-end;
        margin-right: 0.8rem;
        color: #6e6b7b;

        h4{
          font-size: 14px;
        }
        p{
          font-size: 11.5px;
        }
        @include breakpoint(xs){
          display: none;
        }
      }
      img{
        width: 40px;
        height: 40px;
        border-radius: 50%;
      }
      .navbar__right__profile__photo{
        position: relative;
        display: flex;

        .online{
          position: absolute;
          top: 30px;
          right: 0;
          width: 12px;
          height: 12px;
          border-radius: 50%;
          background-color: #28c76f;
          border: 1px solid #fff;
        }
      }
    }
  }

  @include breakpoint(sm) {
    margin: 1.3rem 1rem 0;
  }
}
.burger--menu{
  display: none !important;
  @include breakpoint(lg){
    display: flex !important;
  }

}

.cart{
  position: absolute;
  top: 5rem;
  right: 13.5rem;
  width: 30rem;
  border-radius: $border-radius;
  box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
  display: flex;
  flex-direction: column;
  display: none;
  @include breakpoint(sm){
    right: 0;
    width: 100%;
  }
  &.show{
    display: block;
  }
  &__title{
    padding: 1.22rem 1.28rem;
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #ebe9f1;
    h2{
      color: #5e5873;
      font-weight: 500;
      font-size: 18px;
    }
    &__count{
      padding: 0.3rem 0.5rem;
      border-radius: 20px;
      font-weight: 600;
      font-size: 12px;
      color: $icons;
      background-color: rgba(115,103,240,.12);
    }
  }
  &__items{
    border-bottom: 1px solid #ebe9f1;
    max-height: 20rem;
    overflow-y: scroll;
    &::-webkit-scrollbar {
      display: none;
    }
  
  }
  &__total{
    padding: 1.28rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;

    &__top{
      display: flex;
      justify-content: space-between;

      h4{
        font-weight: 600;
      }
      &__money{
        color: $icons !important;
      }
    }

    &__bot{
      padding: 0.786rem 1.5rem;
      font-weight: 600;
      cursor: pointer;
      color: #fff;
      background-color: $icons;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: $border-radius;
      transition: 0.3s ease-in-out;
      &:hover{
        box-shadow: 0 5px 10px 1px rgba(115, 103, 240, 70%);
      }
    }
  }
  >div {
    // padding: 1rem;
    width: 100%;
  }
  &__empty{
    text-align: center;
    padding: 1rem;
  }
}

.badge{
  font-size: 11px;
  font-weight: 600;
  position: absolute;
  top: -10px;
  right: 0;
  background-color: $icons;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 20px;
  width: 20px;
  padding: 0.3rem 0.5rem;
  border-radius: 20px;
  padding: 0.2rem;

}
</style>