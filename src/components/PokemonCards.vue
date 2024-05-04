<template>
  <div class="w-[15rem] sm:w-[30rem] lg:w-[50rem] m-auto">
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8 lg:gap-4 2xl:gap-6 mt-10 2xl:mt-16">
      <div
        :class="{ 'cursor-pointer': !pokemon.evolved }"
        @click="
          () => {
            if (!pokemon.evolved) {
              $emit('showEvolutionCardsHandler', pokemon.id);
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
    <div class="mt-20 flex justify-center" v-if="evolutionCardsLoading">
      <LoadingSpinner />
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";
import PokemonCard from "./PokemonCard.vue";
import LoadingSpinner from "./LoadingSpinner.vue";

defineProps({
  pokemons: Array,
  evolutionCardsLoading: Boolean,
});
</script>
