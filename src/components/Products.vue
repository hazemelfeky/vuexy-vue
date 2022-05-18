<template>
  <div class='products'>
      <div class="products__left" :class="{ show: showingFilters}" >
        <h2>Filters</h2>
        <div class='products__left__card comp'>
          <div class="products__left__card__filter">
            <h2>Multi Range</h2>
            <div v-for="range in ranges" :key="range.name" class="radio">
              <input type="radio" :id="range.name" name="range" :value="range.value" v-model="rangeState" />
              <label :htmlFor="range.name">{{range.title}}</label>
            </div>
          </div>

          <PriceRange />

          <div class="products__left__card__filter">
            <h2>Categories</h2>
            <div v-for="category in categories" :key="category" class="radio">
              <input type="radio" :id="category" name="categories" :value="category" v-model="categoryState"  />
              <label :htmlFor="category">{{category}}</label>
            </div>
          </div>

          <div class="products__left__card__filter">
            <h2>Brands</h2>
            <div class='radio' :key="brand" v-for="brand in brands">
              <input type="radio" :id="brand" name="brands" :value="brand" v-model="brandsState" />
              <label :htmlFor="brand">{{brand}}</label>
            </div>
          </div>

          <div class="products__left__card__filter">
            <h2>Ratings</h2>
            <div class="stars" v-for="rating in ratings" :key="rating">
              <input hidden type="radio" :id="rating" name="ratings" :value="rating" v-model="ratingState"/>
              <label class="label--stars" :htmlFor="rating">
                <ul>
                  <li v-for="i in 5" :key="i">
                    <StarIcon v-if="i<=rating" :fill="true" />
                    <StarIcon v-else :stroke="true" />
                  </li>
                </ul>
                <span>&up</span>
              </label>
              <div class='stars__count'>{{products.products.filter( product => product.rating == rating || product.rating > rating ).length}}</div>
            </div>
          </div>
        </div>
      </div>


      <div class='products__right'>
        <div class='products__right__header'>
          <div class='products__right__header__left'>
            <h2>{{filteredProducts.length || 'No'}} results found</h2>
            <a href='#' @click="handleShowFilters" class='burger--menu--filter'><svg xmlns="http://www.w3.org/2000/svg" width="21px" height="21px" view-box="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-menu"><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"></line><line x1="3" y1="18" x2="21" y2="18"></line></svg></a>
          </div>
          <div class='products__right__header__right'>
            <Ordering :selectedOder="order" :setSelectedOrder="setOrder" />
            <div class='products__right__header__right__style'>
              <div :class="{active: productGrid}" @click="productGrid = true" >
                <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="18px" height="18px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-grid"><rect data-v-15eba8c6="" x="3" y="3" width="7" height="7"></rect><rect data-v-15eba8c6="" x="14" y="3" width="7" height="7"></rect><rect data-v-15eba8c6="" x="14" y="14" width="7" height="7"></rect><rect data-v-15eba8c6="" x="3" y="14" width="7" height="7"></rect></svg>
              </div>
              <div :class="{active: !productGrid}" @click="productGrid = false">
                <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="18px" height="18px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-list"><line data-v-15eba8c6="" x1="8" y1="6" x2="21" y2="6"></line><line data-v-15eba8c6="" x1="8" y1="12" x2="21" y2="12"></line><line data-v-15eba8c6="" x1="8" y1="18" x2="21" y2="18"></line><line data-v-15eba8c6="" x1="3" y1="6" x2="3.01" y2="6"></line><line data-v-15eba8c6="" x1="3" y1="12" x2="3.01" y2="12"></line><line data-v-15eba8c6="" x1="3" y1="18" x2="3.01" y2="18"></line></svg>
              </div>
            </div>
          </div>
        </div>
        <div class='products__right__search comp'>
          <input v-model="search" placeholder='Search...' class='products__right__search__input' />
          <svg data-v-15eba8c6="" xmlns="http://www.w3.org/2000/svg" width="14px" height="14px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-muted feather feather-search"><circle data-v-15eba8c6="" cx="11" cy="11" r="8"></circle><line data-v-15eba8c6="" x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
        </div>

        <div v-if="productGrid" class='products__right__grid'>
          <ProductGrid v-for="product in shownProducts" :key="product.id" :product="product" />
        </div>
        <div v-else class='products__right__list'>
          <ProductRow v-for="product in shownProducts" :key="product.id" :product="product" />
        </div>
          
        <div class='products__right__pagination'>
          <button :disabled="currentPage == 1" class='products__right__pagination__left' @click="currentPage--">
            <svg xmlns="http://www.w3.org/2000/svg" width="18px" height="18px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-left"><polyline points="15 18 9 12 15 6"></polyline></svg>
          </button>
          <div class='products__right__pagination__list'>
            <button v-for="i in (numberOfPages || 1)" :key="i" @click="currentPage = i" :class="{'active': i == currentPage }">{{i}}</button>
          </div>
          <button :disabled="currentPage >= numberOfPages" class='products__right__pagination__right' @click="currentPage++">
            <svg xmlns="http://www.w3.org/2000/svg" width="18px" height="18px" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-right"><polyline points="9 18 15 12 9 6"></polyline></svg>
          </button>
        </div>
      </div>
    </div>
</template>

<script>
import { computed, onMounted, ref } from 'vue'
import PriceRange from './sub-component/PriceRange.vue'
import ProductGrid from './sub-component/ProductGrid.vue'
import ProductRow from './sub-component/ProductRow.vue'
import Ordering from './sub-component/Ordering.vue'
import StarIcon from './sub-component/StarIcon.vue'

export default {
  components: { PriceRange, ProductGrid, ProductRow, Ordering, StarIcon },
  props: [ 'showingFilters', 'handleShowFilters' ],
  setup(){
    const ranges = [
      { title: 'All', value: {down: 0, up:1000000}, name: 'All'},
      { title: '<= $10', value: {down: 0, up:10}, name: '10'},
      { title: '$10 - $100', value: {down: 10, up:100}, name: '100'},
      { title: '$100 - $500', value: {down: 100, up:500}, name: '500'},
      { title: '>= $500', value: {down: 500, up:1000000}, name: 'more'},
    ]

    const products ={
                      "products":[
                          {
                              "id":1,
                              "name": "Apple Watch Series 5",
                              "price": 339.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/1.3b312012.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":2,
                              "name": "Apple iPhone 11 (64GB, Black)",
                              "price": 669.99,
                              "rating": 5,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/2.1aba2cea.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":3,
                              "name": "Apple iMac 27-inch",
                              "price": 999.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/3.29c766f8.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":4,
                              "name": "OneOdio A71 Wired Headphones",
                              "price": 49.99,
                              "rating": 3,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/4.73f34744.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":5,
                              "name": "Apple - MacBook Air® (Latest Model) - 13.3 Display - Silver",
                              "price": 999.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/5.c5b188e5.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":6,
                              "name": "Switch Pro Controller",
                              "price": 429.99,
                              "rating": 3,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/6.833c8951.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":7,
                              "name": "Google - Google Home - White/Slate fabric",
                              "price": 129.29,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/1.3b312012.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "google",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":8,
                              "name": "Sony 4K Ultra HD LED TV",
                              "price": 7999.99,
                              "rating": 5,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/8.c170e8ca.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":9,
                              "name": "OnePlus 7 Pro",
                              "price": 14.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/9.3170c803.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Philips",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":10,
                              "name": "Logitech K380 Wireless Keyboard",
                              "price": 81.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/10.a197f12f.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby. Apple Watch Series 5 is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Logitech",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":11,
                              "name": "Nike Air Max",
                              "price": 81.99,
                              "rating": 5,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/11.196910d4.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Nike",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":12,
                              "name": "New Apple iPad Pro",
                              "price": 799.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/12.87084176.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "apple",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":13,
                              "name": "Vankyo leisure 3 mini projector",
                              "price": 99.99,
                              "rating": 2,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/13.e7c28d6c.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Vankyo Store",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":14,
                              "name": "Wireless Charger 5W Max",
                              "price": 10.83,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/14.df784ed6.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "3M",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":15,
                              "name": "Laptop Bag",
                              "price": 29.99,
                              "rating": 5,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/15.2b721276.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "TAS",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":16,
                              "name": "Adidas Mens Tech Response Shoes",
                              "price": 54.59,
                              "rating": 5,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/16.a9b3f7ab.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Adidas",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":17,
                              "name": "Oculus Quest All-in-one VR",
                              "price": 645,
                              "rating": 1,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/18.fac01044.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Oculus",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":18,
                              "name": "Handbags for Women Large Designer bag",
                              "price": 39.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/17.024d4a22.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Hobo",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":19,
                              "name": "Giotto 32oz Leakproof BPA Free Drinking Water",
                              "price": 16.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/19.1c1f4cf1.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "3M",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":20,
                              "name": "PlayStation 4 Console",
                              "price": 339.95,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/20.ad629602.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Sony",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":21,
                              "name": "Bugani M90 Portable Bluetooth Speaker",
                              "price": 56,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/21.940a62ff.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Bugani",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":22,
                              "name": "Toshiba Canvio Advance 2TB Portable External Hard Drive",
                              "price": 69.99,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/23.ec286c40.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Toshiba",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":23,
                              "name": "Tile Pro - High Performance Bluetooth Tracker",
                              "price": 29.99,
                              "rating": 3,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/22.450e8e03.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Tile",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":24,
                              "name": "Ronyes Unisex College Bag Bookbags for Women",
                              "price": 23.99,
                              "rating": 2,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/24.79a14740.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Ronyes",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":25,
                              "name": "Willful Smart Watch for Men Women 2020,",
                              "price": 29.99,
                              "rating": 5,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/25.e1f92d21.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Willful",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":26,
                              "name": "VicTsing Wireless Mouse,",
                              "price": 10.99,
                              "rating": 3,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/27.9b4c2313.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "VicTsing",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          },
                          {
                              "id":27,
                              "name": "Bose Frames Tenor - Rectangular Polarized, Bluetooth Audio Sunglasses",
                              "price": 249,
                              "rating": 4,
                              "img": "https://pixinvent.com/demo/vuexy-vuejs-admin-dashboard-template/demo-1/img/26.257af602.png",
                              "discription": "On Retina display that never sleeps, so it’s easy to see the time and other important information, without raising or tapping the display. New location features, from a built-in compass to current elevation, help users better navigate their day, while international emergency calling1 allows customers to call emergency services directly from Apple Watch in over 150 countries, even without iPhone nearby.  is available in a wider range of materials, including aluminium, stainless steel, ceramic and an all-new titanium.",
                              "brand": "Bose",
                              "available": true,
                              "colors": ["red", "purple", "green"]
                          }
                      ]
                  }
    const categories = [ 'Appliances', 'Audio', 'Cameras & Camcorders', 'Car Electronics & GPS', 'Cell Phones',
                        'Computers & Tablets', 'Health, Fitness & Beauty', 'Office & School Supplies', 'TV & Home Theater',
                        'Video Games']
    const brands = [ 'All', 'Insignia™', 'Nike', 'Samsung', 'Google', 'HP', 'Apple', 'Adidas', 'Sony', 'Incipo', 'KitchenAid', 'whirlpool']
    const ratings = [5, 4, 3, 2, 1]
    const itemsPerPage = 9

    const search = ref("")
    const rangeState = ref({down: 0, up:1000000})
    const brandsState = ref('All')
    const ratingState = ref()
    const categoryState = ref(null)
    const order = ref('Featured')
    const currentPage = ref(1)
    const productGrid = ref(true)

    const setOrder = (x) => {
      order.value = x
    }

    const numberOfPages = computed( () => {
      return Math.ceil(filteredProducts.value.length/itemsPerPage)
    })

    const filteredProducts = computed(() => {
      return products.products.filter( product => search.value === ''? product : product.name.toLowerCase().includes(search.value.toLowerCase().trim()) )
        .filter( product => brandsState.value == 'All'? product: product.brand == brandsState.value.toLowerCase() )
        .filter( product => ratingState.value == null? product: product.rating >= ratingState.value )
        .filter( product => product.price < rangeState.value.up && product.price > rangeState.value.down )
        .filter( product => product.available )
        .sort( (x, y) => order.value === "Featured"? x: order.value === "Highest"? y.price - x.price : x.price - y.price)
    })

    const shownProducts = computed( () => {
      const indexOfLastItem = (currentPage.value) * itemsPerPage
      const indexOfFirstItem = indexOfLastItem - itemsPerPage
      return filteredProducts.value.slice(indexOfFirstItem, indexOfLastItem)
    })

    return {search, currentPage, ratingState, order, itemsPerPage, brandsState, rangeState, numberOfPages,  shownProducts, productGrid, products, filteredProducts, ranges, categoryState, categories, brands, ratings, setOrder}
  }
}
</script>

<style lang="scss">
@import '../styles/varibles.scss';
.label--stars{
  display: flex;
}
ul{
  list-style: none;
  padding: 0;
  display: flex;
}

.products{
  display: flex;
  gap: 2rem;
  padding-bottom: 2rem;

  h2{
    font-size: 1rem;
    font-weight: 500;
    line-height: 1.2;
    color: #5e5873;
    margin-bottom: 1.75rem;
    
  }

  &__left{
    @include breakpoint(md){
      position: fixed;
      top: 0;
      left: -$drawer--width;
      transition: tranform 0.5s ease-in-out;
      z-index: 500;
      &__card{
        border-radius: 0px !important;
        overflow: scroll;
        height: 100vh;
      }
      &.show{
        transform: translateX($drawer--width);
      }
      >h2{
        display: none;
      }
    }
    
    &__card{
      width: $drawer--width;
      display: flex;
      flex-direction: column;
      
      gap:2.2rem;
      box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
      border-radius: $border-radius;
      padding: 21px;
      padding-bottom: 6rem;

      h2{
        margin-bottom: 1rem;
      }
      .stars{
        display: flex;
        justify-content: space-between;
        span{
          color: $icons;
        }
      }
    }
  }

  &__right{
    width: -webkit-fill-available;

    .burger--menu--filter{
      display: none;
      @include breakpoint(md){
        display: flex;
      }
    }
    &__header{
      display: flex;
      justify-content: space-between;
      margin-bottom: 0.6rem;
      &__right{
        display: flex;
        gap: 1rem;

        &__style{
          display: flex;
          color: $icons;
          border: 1px solid $icons;
          border-radius: 5px;

          div{
            display: flex;
            padding: 0.5rem 1rem;
            cursor: pointer;
            &:first-child{
              border-right: 1px solid $icons;
            }
            &:hover{
              background-color: rgba(115,103,240,.05);
            }
            &.active{
              background-color: rgba(115,103,240,.2);
            }
          }
        }
      }

      &__left{
        display: flex;
        align-items: center;
        h2{
          margin-bottom: 1rem;
        }
        @include breakpoint(md){
          h2{
            display: none;
          }
          a{
            padding-bottom: 1rem;
          }
        }
      }
    }
    &__search{
      border-radius: $border-radius;
      box-shadow: 0 4px 24px 0 RGB(34 41 47 / 10%);
      height: 48px;
      display: flex;
      align-items: center;
      font-size: .95rem;
      color: #b4b7bd;
      padding: 1rem 1.25rem;
      gap: 1rem;

      input{
        background-color: inherit;
        outline: none;
        border: none;
        width: -webkit-fill-available;
      }
    }

    &__grid{
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
    &__list{
      margin-top: 2rem;
      display: flex;
      flex-direction: column;
      gap: 2rem;
    }
    &__pagination{
      display: flex;
      gap: 0.35rem;
      justify-content: center;
      margin-top: 1rem;
      &>button{
        &:hover:not(:disabled){
          background-color: $icons;
          color: #fff;
        }
        &:disabled{
          cursor: auto;
          background-color: #f3f2f7;
          border-color: #dae1e7;
        }
      }
      button{
        border: none;
        cursor: pointer;
        height: 32px;
        width: 32px;
        border-radius: 50%;
        display: grid;
        place-items: center;
        color: $text--light;
        transition: all 0.2s ease;
        // background-color: ;
      }
      &__list{
        display: flex;
        color: $text--light;
        background-color: #fff;
        border-radius: 20px;

        .active{
          background-color: $icons !important;
          color: #fff;
          font-weight: bold;
        }
      }

    }
  }
}

.radio{
  margin-bottom: 0.75rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

input[type='radio']:after {
  width: 15px;
  height: 15px;
  border-radius: 15px;
  top: -3px;
  left: -2px;
  position: relative;
  background-color: $comp--dark;
  content: '';
  display: inline-block;
  visibility: visible;
  border: 1px solid #b8c0d354;
}

input[type='radio']:checked:after {
  width: 15px;
  height: 15px;
  border-radius: 15px;
  top: -2px;
  left: -1px;
  position: relative;
  background-color: $icons !important;
  border: none;
  box-shadow: 0 2px 4px 0 rgba(115,103,240,.4) ;
  content: '';
  display: inline-block;
  visibility: visible;
  // border: 2px solid white;
}
</style>