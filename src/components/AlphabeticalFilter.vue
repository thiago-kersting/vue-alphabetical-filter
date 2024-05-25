<template>
  <div id="filter__container">
      <ul ref="list" @click="moveSlider">
          <button
          v-for="item in nameMap.keys()" :key="item"
          @click="moveSlider"
          ref="items"
          >
              <li>{{ item }}</li>
          </button>
      </ul>
      <div class="slider"
           @mousedown="dragStart" @touchstart="dragStart"
           @mousemove="drag" @touchmove="drag"
           @mouseup="dragEnd" @touchend="dragEnd"
           @mouseleave="dragEnd">
      </div>
  </div>
</template>

<script setup>
import { defineProps, ref, reactive, onMounted } from 'vue';

const props = defineProps({
  sendMap: {
      type: Function,
      required: true,
  },
  sendKey: {
      type: Function,
      required: true,
  },
  arrayList: {
      type: Array,
      required: true,
  },
})
const arrayListSort = [...props.arrayList].sort();

function noAccentuation(name) {
return name.normalize('NFD').replace(/[\u0300-\u036f]/g, '');
}
const nameMap = new Map();
arrayListSort.forEach(name => {
  const nameNoAccentuation = noAccentuation(name);
  const firstLetter = nameNoAccentuation.charAt(0).toUpperCase();

  if (!nameMap.has(firstLetter)) {
      nameMap.set(firstLetter, [name]);
  } else {
      nameMap.get(firstLetter).push(name);
  }
});

const list = ref(null);
const items = ref([]);
const state = reactive({ currentItem: null });

let dragging = false;
let slider;
let container;

onMounted(() => {
  slider = document.querySelector('#filter__container .slider');
  container = document.querySelector('#filter__container');
  props.sendMap(nameMap)
});

const dragStart = (e) => {
  document.body.style.overflow = 'hidden';
  dragging = true;
  slider = e.target;
  container = slider.parentElement;
};

const drag = (e) => {
  if (!dragging) return;
  const rect = container.getBoundingClientRect();
  const listRect = list.value.getBoundingClientRect();
  const clientY = e.clientY || (e.touches && e.touches.length > 0 ? e.touches[0].clientY : 0); // Verifica se e.touches existe e tem comprimento maior que zero
  const y = clientY - rect.top - slider.offsetHeight / 2;
  if (y >= 0 && y <= listRect.height - slider.offsetHeight) {
      slider.style.top = `${y}px`;
  }
  adjustSliderPosition();
};


const dragEnd = () => {
  document.body.style.overflow = '';
  dragging = false;
};

const moveSlider = (e) => {
  const rect = container.getBoundingClientRect();
  const listRect = list.value.getBoundingClientRect();
  const y = (e.clientY || e.touches[0].clientY) - rect.top - slider.offsetHeight / 2;
  if (y >= 0 && y <= listRect.height - slider.offsetHeight) {
      slider.style.top = `${y}px`;
  }
  adjustSliderPosition();
};

const adjustSliderPosition = () => {
  const sliderTop = slider.offsetTop;
  const closestItem = items.value.reduce((prev, curr) =>
      Math.abs(curr.offsetTop - sliderTop) < Math.abs(prev.offsetTop - sliderTop) ? curr : prev
  );
  slider.style.top = `${closestItem.offsetTop}px`;
  const closestItemText = closestItem.textContent.trim();
  if (state.currentItem !== closestItemText) {
      state.currentItem = closestItemText;
      props.sendKey(state.currentItem)
  }
};
</script>

<style>
#filter__container {
  position: relative;
  border-radius: 1rem;
}
#filter__container ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: max-content;
}
#filter__container button {
  background: none;
  border: none;
  width: 20px;
  height: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
#filter__container li {
  color: var(--secondary_00-color);
  -webkit-touch-callout: none;
 -webkit-user-select: none;
 -khtml-user-select: none;
 -moz-user-select: none;
 -ms-user-select: none;
 user-select: none;
}
#filter__container .slider {
  position: absolute;
  width: 20px;
  height: 20px;
  background: var(--secondary_00-color);
  opacity: 0.2;
  border-radius: 1rem;
  cursor: pointer;
  top: 0;
  background: blue;
}
</style>

