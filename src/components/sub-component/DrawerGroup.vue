<template>
  <div class="drawer--group">
      <h3>{{groupName}}</h3>
      <DrawerLink v-for="link in links" :key="link.name" :link="link" />
    </div>
</template>

<script>
import DrawerLink from "./DrawerLink.vue"

export default {
  name:"DrawerGroup",
  props: ['groupName', 'links'],
  components: { DrawerLink }
}
</script>

<style lang="scss">
  .drawer--group{

    h3{
      color: #a6a4b0;
      margin: 2.486rem 0 1.2rem 2.2rem;
      font-size: 0.9rem;
      font-weight: 500;
    }
  }
</style>