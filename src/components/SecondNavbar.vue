<template>
  <div class='second--nav'>
    <div class='second--nav__left'>
      <h3>Shop</h3>
    </div>
    <div class='second--nav__right'>
      <a href='#'><svg xmlns="http://www.w3.org/2000/svg" width="16px" height="16px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="align-text-top feather feather-home"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg></a>
      <div class="arrow-right"></div>
      <a href='#'>ECommerce</a>
      <div class="arrow-right"></div>
      <p>Shop</p>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style scoped lang="scss">
  @import '../styles/varibles.scss';

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

  .second--nav{
    margin: 2rem 0 2.7rem;
    display: flex;
    align-items: center;
    gap: 0.4rem;

    &__left{
      border-right: 1px solid #d6dce17a;
      padding-right: 1rem;

      h3{
        font-weight: 500;
        font-size: 24px;
      }
    }

    &__right{
      padding-left: 0.8rem;
      display: flex;
      gap: 0.4rem;
      align-items: center;

      a{
        color: $icons !important;
        display: flex;
      }
    }
    
  }

</style>