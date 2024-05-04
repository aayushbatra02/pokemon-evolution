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
import { usePokemonFetch } from "@/composables/use-fetch";

const {
  showEvolutionCardsHandler,
  cardsLoading,
  evolutionCardsLoading,
  pokemons,
} = usePokemonFetch();
</script>