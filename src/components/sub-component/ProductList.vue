<template>
  <div className=''>
      <div className='list'>
      </div>
      <div className='navigator'>
        {/* { slicedProduscts.length.map( page => <div key={page} className='list__navigator__page'>{page + 1}</div>)} */}
      </div>
    </div>
</template>

<script>
export default {
  name:"ProductList"
}
</script>

<style lang="scss">
@import '../../styles/varibles.scss';

.list{
  margin-top: 2rem;
  display: grid;
  grid-template-columns: repeat( 3, 1fr );
  gap: 2rem;

  @include breakpoint(md){
    grid-template-columns: repeat( 2, 1fr );
  }
  @include breakpoint(xs){
    grid-template-columns: repeat( 1, 1fr );
  }
}
</style>