<template>
  <div @click="open = !open">
    <div class="drawer--link" :class="{ 'active': active, 'super--active': superActive}">
      <div class='drawer--link__container'>
        <a href="#">
          <svg v-if="!isSub" class="drawer--link__icon feather feather-file-text" xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path><polyline points="14 2 14 8 20 8"></polyline><line x1="16" y1="13" x2="8" y2="13"></line><line x1="16" y1="17" x2="8" y2="17"></line><polyline points="10 9 9 9 8 9"></polyline></svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather-circle drawer--link__icon feather "><circle cx="12" cy="12" r="10"></circle></svg>
          <p>{{link?.name}}</p>
        </a>
      </div>
      <div v-if="link?.arrow" class="arrow-right" :class="{open}"></div>
    </div>
    <div class="drawer--sublinks__container" :class="{open}" >
      <div v-if="link?.subLinks">
        <DrawerLink v-for="sub in link.subLinks" :key="sub.name" :link="{...sub}" :isSub="true" />
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  name:"DrawerLink",
  props: [ 'link', 'isSub' ],
  setup(props) {
    const open = ref(false)
    const {active, superActive} = props.link
    // console.log(props.link.active);
    // console.log(props.link?.name);

    return { open, active, superActive }
  }
}
</script>

<style scoped lang="scss">
  @import '../../styles/varibles.scss';

  .arrow-right{
    display: block;
    margin-right: 10px;
    width: 7px;
    height: 7px;
    border-top: 1px solid #000;
    border-left: 1px solid #000;
    transform: rotate(135deg);
    transition: all 0.3s ease;
    border-color: $text--light;
  }
  .arrow-right.open{
    transform: rotate(225deg);
  }

  .feather-circle{
    height: auto !important;
    width: auto !important;
  }

  .drawer--link{
    display: flex;
    align-items: center;
    margin: 0 0.6rem 0 1rem;
    border-radius: $border-radius;

    &.active{
      background-color: $container--light;
    }

    &.super--active{
      background: linear-gradient(118deg,#7367f0,rgba(115,103,240,.7));
      box-shadow: 0 0 10px 1px rgba(115, 103, 240, 70%);
      color: $container--light;
      margin-top: 0.4rem;
    }

    &__container{
      width: -webkit-fill-available;
      a{
        color: inherit;
        text-decoration: none;
        display: flex;
        // margin: 0 15px;
        padding: 10px 15px 10px 15px;
        transition: transform 0.3s ease-in-out;
        display: flex;
        align-items: center;
        
        .drawer--link__icon{
          width: 1.45rem;
          height: 1.45rem;
          margin-right: 1.1rem;

          &.circle{
            font-size: 0.7rem;
            width: 22.39px;
          }
        }
        p{
          height: 22.35px;
        }
      }
      >:hover{
        transform: translateX(8px);
      }
    }
    .drawer--link__arrow{
      font-size: 1rem;
      margin-right: 0.6rem;
      z-index: 5;
    }
  }

  .drawer--sublinks__container{
    // transition: 0.5s ease-in-out;
    // transform: scaleY(0);
    // transform-origin: top center;
    display: none;

    &.open{
      display: block;
      // transform: scaleY(1);
    }
  }

  @keyframes growDown {
    0% {
        transform: scaleY(0)
    }
    100% {
        transform: scaleY(1)
    }
  }
</style>