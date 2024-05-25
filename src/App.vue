<template>
 <section class="container" style="position: relative">
      <div class="filter">
          <AlphabeticalFilter :sendKey="scrollToKey" :sendMap="handleUpdateMap" :array-list="array" />
      </div>
      <div style="display: flex; flex-direction: column" v-for="[key, value] in mapList" :key="key" :id="key">
          {{ key }}
          <div v-for="name in value" :key="name">
              {{ name }}
          </div>
      </div>
  </section>
</template>

<script setup>
import AlphabeticalFilter from './components/AlphabeticalFilter.vue';
import { ref } from 'vue';
// array for the alphabetical filter
const array = [
  "Bruno", "Clara", "Daniel", "Eduarda",
  "Fernando", "Gabriela", "Henrique", "Isabela",
  "Kátia", "Lucas", "Maria", "Nuno", "Olívia",
  "Paulo", "Quiteria", "Rafael", "Sofia", "Tiago",
  "Úrsula", "Vitor", "Wanda", "Xavier", "Yasmin",
  "Zara", "André", "Bianca", "Carlos", "Débora",
  "Elias", "Fátima", "Gustavo", "Helena", "Ícaro",
  "Kleber", "Larissa", "Miguel", "Natália",
  "Otávio", "Pâmela", "Quintino", "Renata", "Samuel",
  "Tatiana", "Ulisses", "Valentina", "William"
]

const mapList = ref(new Map());

// receive the maplist
const handleUpdateMap = (newMap) => {
    mapList.value = newMap;
}
// function to go to key in filter
function scrollToKey(key) {
    const element = document.querySelector(`#${key}`);
    if (element) {
        const rect = element.getBoundingClientRect();
        const offset = 0; // subsitua pelo tamanho do seu header;
        const targetPosition = rect.top + window.scrollY + offset;
        window.scrollTo({
            top: targetPosition,
            behavior: 'smooth'
        });
    }
}
</script>

<style>
.container {
  display:flex;
  flex-direction: column;
  gap: 1rem;
  justify-content: center;
  align-items: flex-start;
  margin-top: 1rem;
}
.filter {
  position: fixed;
  right: 12px;
  top: 50%;
  transform: translateY(-40%);
}
</style>