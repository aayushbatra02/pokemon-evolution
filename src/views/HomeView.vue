<template>
  <div>
    <PokemonCards
      :pokemons="pokemons"
      @showEvolutionHandler="showEvolutionHandler"
    />
  </div>
</template>

<script setup>
import PokemonCards from "@/components/PokemonCards.vue";
import { onMounted, ref } from "vue";

const pokemons = ref([]);

const fetchPokemon = async (url) => {
  try {
    const res = await fetch(url);
    if (res.status === 200) {
      const data = await res.json();
      const pokemonTypes = data.types.map((ele) => ele.type.name);
      const pokemon = {
        id: data.id,
        name: data.name,
        types: pokemonTypes,
        image: data.sprites.other["official-artwork"].front_default,
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
  const pokemonEvolved1 = await fetchPokemon(
    `https://pokeapi.co/api/v2/pokemon/${id + 1}`
  );
  const pokemonEvolved2 = await fetchPokemon(
    `https://pokeapi.co/api/v2/pokemon/${id + 2}`
  );
  pokemonEvolved1.evolved = true;
  pokemonEvolved2.evolved = true;
  pokemons.value.push(pokemonEvolved1, pokemonEvolved2);
};

onMounted(async () => {
  try {
    const responses = await Promise.all([
      fetchPokemon("https://pokeapi.co/api/v2/pokemon/1"),
      fetchPokemon("https://pokeapi.co/api/v2/pokemon/4"),
      fetchPokemon("https://pokeapi.co/api/v2/pokemon/7"),
    ]);
    pokemons.value = responses;
  } catch (error) {
    console.error("Error:", error);
  }
});
</script>