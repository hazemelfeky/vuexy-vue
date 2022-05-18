<template>
  <div class='price--range'>
    <h2>Price Range</h2>
    <div class="middle">
      <div class="multi-range-slider">
        <input type="range" v-model="left" min="0" max="100" />
        <input type="range" v-model="right" min="0" max="100" />
        <!-- <input type="range" v-model="right" @change="right.value = $event" :ref="inputRight" min="0" max="100" :value="right" /> -->

        <div class="slider">
          <div class="track"></div>
          <div id="range" class="range"></div>
          <div id="thumbLeft" class="thumb left"></div>
          <div id="thumbRight" class="thumb right"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { onUpdated, ref } from 'vue'
export default {
  name:"PriceRange",
  setup() {
    const left = ref(10)
    const right = ref(90)
		const range = document.getElementById('range')
		const thumbLeft = document.getElementById('thumbLeft')
		const thumbRight = document.getElementById('thumbRight')

		const setLeftValue = () => {
			left.value =  Math.min(left, right - 1)

			thumbLeft.current.style.left = left + "%";
			range.current.style.left = left + "%";
		}

		const setRightValue = () => {
			right.value =  Math.min(right, left + 1)

			thumbRight.current.style.right = (100 - right) + "%";
			range.current.style.right = (100 - right) + "%";
		}

		onUpdated( () => {
			setLeftValue()
			setRightValue()
		})

    return {left, right}
  }
}
</script>

<style lang="scss">
@import '../../styles/varibles.scss';

.middle {
	position: relative;
	max-width: 500px;
}

.slider {
	position: relative;
	z-index: 1;
	height: 4px;
	margin: 0 15px;
}
.slider > .track {
	position: absolute;
	z-index: 1;
	left: -15px;
	right: 0;
	top: 0;
	bottom: 0;
	border-radius: 5px;
	background-color: #c7b2e533;
}
.slider > .range {
	position: absolute;
	z-index: 2;
	left: 23%;
	right: 23%;
	top: 0;
	bottom: 0;
	border-radius: 5px;
	background-color: $icons;
}
.slider > .thumb {
	position: absolute;
	z-index: 3;
	top: 5px;
	width: 14px;
	height: 14px;
	background-color: #fff;
	border: 2px solid $icons;
	border-radius: 50%;
	box-shadow: 0 0 0 0 rgba(98,0,238,.1);
	transition: box-shadow .3s ease-in-out;
}
.slider > .thumb.left {
	left: 25%;
	transform: translate(-15px, -10px);
}
.slider > .thumb.right {
	right: 25%;
	transform: translate(15px, -10px);
}

input[type=range] {
	position: absolute;
	pointer-events: none;
	-webkit-appearance: none;
	z-index: 2;
	height: 10px;
	width: 100%;
	opacity: 0;
}
input[type=range]::-webkit-slider-thumb {
	pointer-events: all;
	width: 30px;
	height: 30px;
	border-radius: 0;
	border: 0 none;
	background-color: red;
	-webkit-appearance: none;
}

</style>