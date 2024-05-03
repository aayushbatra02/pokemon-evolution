<template>
  <div class="grid grid-cols-3 gap-4 mt-10 w-[50rem] m-auto">
    <div
      @click="
        () => {
          if (pokemon.evolved) {
            console.log('Pokemon already evolved');
          } else {
            $emit('showEvolutionHandler', pokemon.id);
          }
        }
      "
      v-for="pokemon in pokemons"
      :key="pokemon.id"
    >
      <PokemonCard :pokemon="pokemon">
        <template #title>
          <span>{{ pokemon.name }} #{{ pokemon.id }}</span>
        </template>
        <template #content>
          <img :src="pokemon.image" :alt="`${pokemon.name} image`" />
        </template>
        <template #footer>
          <div v-for="(type, id) in pokemon.types" :key="id">
            {{ type }}
          </div>
        </template>
      </PokemonCard>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";
import PokemonCard from "./PokemonCard.vue";

defineProps({
  pokemons: Array,
});
</script>
