<template>
  <div @click="scrollToTop" class='scroll' :style="{display: visible ? 'flex' : 'none'}">
    <svg data-v-8314f794="" xmlns="http://www.w3.org/2000/svg" width="16px" height="16px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-arrow-up"><line data-v-8314f794="" x1="12" y1="19" x2="12" y2="5"></line><polyline data-v-8314f794="" points="5 12 12 5 19 12"></polyline></svg>
  </div>
</template>

<script>
import { ref } from 'vue';
export default {
  setup(){
    const visible = ref(false)

    const toggleVisible = () => {
      const scrolled = document.documentElement.scrollTop;
      if (scrolled > 300){
        visible.value = true
      }
      else if (scrolled <= 300){
        visible.value = false
      }
    };

    const scrollToTop = () =>{
      window.scrollTo({
      top: 0,
      behavior: 'smooth'
      /* you can also use 'auto' behaviour
        in place of 'smooth' */
      });
    };

    window.addEventListener('scroll', toggleVisible);

    return { visible, scrollToTop }
  }
}
</script>

<style lang="scss">
@import '../styles/varibles.scss';

.scroll{
  position: fixed;
  bottom: 3rem;
  right: 3rem;
  padding: 1rem;
  background-color: $icons;
  border-radius: $border-radius;
  color: $container--light;

}
</style>