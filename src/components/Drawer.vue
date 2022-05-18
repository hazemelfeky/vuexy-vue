<template>
  <div class="drawer comp" :class="{ 'show': showDrawer}">
    <div class='drawer__logo'>
      <a href='#' class='drawer__logo__link'>
        <img src='/logo.36f34a9f.svg' alt='logo'/>
        <h1>Vuexy</h1>
      </a>
      <a @click="closeDrawer" class='drawer__logo__close' href='#'><svg xmlns="http://www.w3.org/2000/svg" width="20px" height="20px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="d-block d-xl-none feather feather-x"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg></a>
    </div>

    <div class='drawer__links'>
      <DrawerLink :link="{name: 'Dashboards', arrow: true}" />
      <DrawerGroup v-for="group in groups" :key="group.groupName" :groupName="group.groupName" :links="group.links" />
    </div>
  </div>
</template>

<script>
import DrawerLink from "./sub-component/DrawerLink.vue"
import DrawerGroup from "./sub-component/DrawerGroup.vue"

export default {
  name:"Drawer",
  components: { DrawerLink, DrawerGroup },
  props: [ 'closeDrawer', 'showDrawer' ],
  setup(props){
    const groups = [
      { groupName: "APPS & PAGES", links: [
        {name: "Email"},
        {name: "Chat"},
        {name: "Todo"},
        {name: "Calendar"},
        {name: "Invoice", arrow: true},
        {name: "eCommerce", arrow: true, active: true, subLinks: [
          {name: "Shop", superActive: true},
          {name: "Details"},
          {name: "Wishlist"},
          {name: "Checkout"},
        ]},
        
        {name: "User", arrow: true},
        {name: "Pages", arrow: true},
      ]},
      { groupName: "USER INTERFACE", links: [
        {name: "Typography"},
        {name: "Colors"},
        {name: "Feather"},
        {name: "Cards"},
        {name: "Components"},
        {name: "Extensions"},
        {name: "Page Layout"},
      ]},
      { groupName: "SECOND USER INTERFACE", links: [
        {name: "Typography"},
        {name: "Colors"},
        {name: "Feather"},
        {name: "Cards"},
        {name: "Components"},
        {name: "Extensions"},
        {name: "Page Layout"},
      ]}
    ]

    return {groups}
  }
  
}
</script>

<style lang="scss">
  @import '../styles/varibles.scss';

  .drawer {
    width: $drawer--width;
    height: 100vh;
    background-color: $comp--light;
    font-size: $drawer--font;
    position: fixed;
    top: 0;
    box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
    // position: relative;
    @include breakpoint(lg){
      position: fixed;
      left: -260px;
      transition: transform 0.2s ease-in-out;
      z-index: 500;
    }

    &.show{
      transform: translateX(260px);
    }

    ::-webkit-scrollbar { /* WebKit */
      width: 6px;
      // transition: all .8s ease-in-out;
    }

    ::-webkit-scrollbar-track {
      background-color: transparent;
      border-radius: 10px;
      transition: background-color .2s linear,width .2s ease-in-out;
    }

    ::-webkit-scrollbar-track:hover {
      background-color: #3b4253;
      width: 11px;
    }

    ::-webkit-scrollbar-thumb {
      background-color: transparent;
      border-radius: 10px;
      // transition: all .5s ease-in-out;
    }

    &:hover ::-webkit-scrollbar-thumb {
      background-color: #b4b7bd;
    }

    &__logo {
      display: flex;
      justify-content: space-between;
      align-items: center;

      &__link {
        color: $icons;
        text-decoration: none;
        display: flex;
        margin-top: 1.35rem;
        margin-bottom: 1rem;
        padding: 0.15rem 1rem 0.3rem 1.44rem;
        padding: 0.35rem 1rem 0.3rem 1.64rem;;
    
        img {
          width: 36px;
        }
        h1 {
          font-size: 20.3px;
          font-weight: 600;
          padding-left: 1rem;
    
        }
      }

      &__close{
        display: none;
        padding-right: 0.94rem;
        padding-top: 1.1rem;
        color: $icons;
        @include breakpoint(lg){
          display: block;
        }
      }
    }

    &__links{
      overflow: scroll;
      height: -webkit-fill-available;
    }
  }
</style>