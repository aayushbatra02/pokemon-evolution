<template>
  <div>
    <component
      :is="LoadingSpinner"
      v-if="cardsLoading"
      class="fixed inset-0 flex justify-center items-center"
    >
    </component>
    <PokemonCards
      v-else
      :pokemons="pokemons"
      :evolutionCardsLoading="evolutionCardsLoading"
      @showEvolutionCardsHandler="showEvolutionCardsHandler"
    />
  </div>
</template>

<script setup>
import LoadingSpinner from "@/components/LoadingSpinner.vue";
import PokemonCards from "@/components/PokemonCards.vue";
import { useFetch } from "@/composables/fetch";

const {
  showEvolutionCardsHandler,
  cardsLoading,
  evolutionCardsLoading,
  pokemons,
} = useFetch();
</script>