<template>
  <div>
    <div
      v-if="cardsLoading"
      class="fixed inset-0 flex justify-center items-center"
    >
      <LoadingSpinner />
    </div>
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

import { onMounted, ref } from "vue";

const pokemons = ref([]);
const cardsLoading = ref(false);
const evolutionCardsLoading = ref(false);

const fetchPokemon = async (url) => {
  try {
    const res = await fetch(url);
    if (res.status === 200) {
      const data = await res.json();
      const pokemonTypes = data?.types.map((ele) => ele.type.name);
      const pokemon = {
        id: data?.id,
        name: data?.name,
        types: pokemonTypes,
        image: data?.sprites?.other["official-artwork"]?.front_default,
      };
      return pokemon;
    }
  } catch (e) {
    console.log(e);
  }
};

const showEvolutionCardsHandler = async (id) => {
  pokemons.value = pokemons.value.filter((pokemon) => !pokemon.evolved);
  try {
    evolutionCardsLoading.value = true;
    const evolutionIds = [id + 1, id + 2];
    const responses = await Promise.all(
      evolutionIds.map((id) =>
        fetchPokemon(`https://pokeapi.co/api/v2/pokemon/${id}`)
      )
    );
    const evolvedPokemons = responses.map((pokemon) => {
      pokemon.evolved = true;
      return pokemon;
    });
    pokemons.value.push(...evolvedPokemons);
  } catch (error) {
    console.error("Error:", error);
  } finally {
    evolutionCardsLoading.value = false;
  }
};

onMounted(async () => {
  try {
    cardsLoading.value = true;
    const pokemonIds = [1, 4, 7];
    const responses = await Promise.all(
      pokemonIds.map((id) =>
        fetchPokemon(`https://pokeapi.co/api/v2/pokemon/${id}`)
      )
    );
    pokemons.value = responses;
  } catch (error) {
    console.error("Error:", error);
  } finally {
    cardsLoading.value = false;
  }
});
</script>