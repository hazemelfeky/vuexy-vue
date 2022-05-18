<template>
  <div @focusout="showOrder = false" tabindex="0" class="selector" :class="{ 'active' : showOrder }">
    <div @click="showOrder = !showOrder" class='selector__head'>
      <h3 class='selector__head__title'>
        {{selectedOder}}
      </h3>
      <div class="arrow-right" ></div>
    </div>
    <div class="selector__choices" :class="{ 'show' : showOrder }">
      <div v-for="ord in orders" :key="ord" class='selector__choices__choice' @click="handleClickOrder(ord)">
        {{ord}}
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  props: [ 'selectedOder', 'setSelectedOrder' ],
  setup(props){
    const showOrder = ref(false)
    const orders = ['Featured', 'Lowest', 'Highest']
    const handleClickOrder = ord => {
      props.setSelectedOrder(ord)
      showOrder.value = false
    }

    return {showOrder, orders, handleClickOrder}
  }

}
</script>

<style scoped lang="scss">
@import '../../styles/varibles.scss';

.arrow-right{
  display: block;
  width: 7px;
  height: 7px;
  border-top: 1px solid #000;
  border-left: 1px solid #000;
  transition: all 0.3s ease;
  border-color: $text--light;
  transform: rotate(225deg);
}

.selector{
  position: relative;
  &.active{
    background-color: rgba(115,103,240,.2);
  }
  &:hover{
    background-color: rgba(115,103,240,.05);
  }
  &__head{
    display: flex;
    gap: 0.5rem;
    align-items: center;
    padding: 0.5rem 0.7rem 0.5rem 1rem ;
    border: 1px solid $icons;
    color: $icons;
    border-radius: $border-radius;
    cursor: pointer;
    h3{
      font-weight: normal;
      user-select: none;
      font-size: 1rem;
    }
  }
  &__choices{
    display: none;
    position: absolute;
    // bottom: -5px;
    right: 0;
    margin-top: 0.4rem;
    border-radius: $border-radius;
    box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
    padding: 0.4rem 0;
    width: 10rem;
    // height: 7.5rem;
    background-color: #fff;
    z-index: 5;
    &.show{
      display: block;
    }

    &__choice{
      padding: 0.65rem 1.28rem;
      user-select: none;
      cursor: pointer;
      &:hover{
        background-color: rgba(115,103,240,.12);
        color: $icons;
      }
    }
  }
}
</style>