<template>
  <Header @toggleMenu="toggleMenu()"></Header>
  <div class="pt-[64px] dark:bg-dark-1 bg-white text-black overflow-auto dark:text-white" style="height: 100vh">
    <div class="w-[80%] m-auto">
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { createToast } from 'mosha-vue-toastify';
import { usePokedexStore } from '@/stores/pokedex';
import localStorage from '@/composables/localStorage';
import Header from '@/components/Header.vue';

const pokedexStore = usePokedexStore();
const { getFilteredPokemons } = pokedexStore;

const menuOpen = ref<boolean>(false);

const storage = localStorage();
const { filters } = storage;

const toggleMenu = (bool: boolean) => (menuOpen.value = bool);

onMounted(async () => {
  try {
    await getFilteredPokemons(filters.value.search, filters.value.types);
  } catch (e: any) {
    createToast('Error while fetching data', { type: 'danger', position: 'bottom-right' });
  }
});
</script>

<style>
html.dark {
  color-scheme: dark;
}
</style>
