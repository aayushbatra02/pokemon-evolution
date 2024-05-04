<template>
  <div>
    <div
      v-if="mainLoading"
      class="fixed inset-0 flex justify-center items-center"
    >
      <LoadingSpinner />
    </div>
    <PokemonCards
      v-else
      :pokemons="pokemons"
      :evolutionLoading="evolutionLoading"
      @showEvolutionHandler="showEvolutionHandler"
    />
  </div>
</template>

<script setup>
import LoadingSpinner from "@/components/LoadingSpinner.vue";
import PokemonCards from "@/components/PokemonCards.vue";

import { onMounted, ref } from "vue";

const pokemons = ref([]);
let mainLoading = ref(false);
let evolutionLoading = ref(false);

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

const showEvolutionHandler = async (id) => {
  const mainPokemons = pokemons.value.filter((pokemon) => !pokemon.evolved);
  pokemons.value = mainPokemons;
  try {
    evolutionLoading.value = true;
    const responses = await Promise.all([
      fetchPokemon(`https://pokeapi.co/api/v2/pokemon/${id + 1}`),
      fetchPokemon(`https://pokeapi.co/api/v2/pokemon/${id + 2}`),
    ]);
    const evolvedPokemons = responses.map((pokemon) => {
      pokemon.evolved = true;
      return pokemon;
    });
    pokemons.value.push(...evolvedPokemons);
  } catch (error) {
    console.error("Error:", error);
  } finally {
    evolutionLoading.value = false;
  }
};

onMounted(async () => {
  try {
    mainLoading.value = true;
    const responses = await Promise.all([
      fetchPokemon("https://pokeapi.co/api/v2/pokemon/1"),
      fetchPokemon("https://pokeapi.co/api/v2/pokemon/4"),
      fetchPokemon("https://pokeapi.co/api/v2/pokemon/7"),
    ]);
    pokemons.value = responses;
  } catch (error) {
    console.error("Error:", error);
  } finally {
    mainLoading.value = false;
  }
});
</script>