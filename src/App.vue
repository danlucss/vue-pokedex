<script setup>
import { onMounted, ref, computed } from "vue";

const search = ref("");
const filteredPokemon = computed(() =>
    pokemonList.value.filter(pokemon => pokemon.pokemon_species.name.includes(search.value))
);

const pokemonList = ref([]);
console.log(pokemonList);

onMounted(async () => {
    const pokeData = await fetch("https://pokeapi.co/api/v2/pokedex/2/").then(res => res.json());
    pokemonList.value = pokeData.pokemon_entries;
});
</script>

<template>
    <h1> My pokedex </h1>

    <input type="text" v-model="search" placeholder="Search for a pokemon" />

    <ul>
        <li v-for="(pokemon, index) in filteredPokemon" :key="`poke-${index}`">
            #{{ pokemon.entry_number }} - {{ pokemon.pokemon_species.name }}
        </li>
    </ul>
</template>

<style scoped>
.logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
}
.logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
    filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
