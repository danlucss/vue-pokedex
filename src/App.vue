<script setup>
import { onMounted, ref, computed, reactive } from "vue";

const search = ref("");

const pokemonStore = reactive({
    list: [],
    filteredList: computed(() =>
        pokemonStore.list.filter(pokemon => pokemon.pokemon_species.name.includes(search.value))
    ),
});

onMounted(async () => {
    const pokeData = await fetch("https://pokeapi.co/api/v2/pokedex/2/").then(res => res.json());

    pokemonStore.list = pokeData.pokemon_entries;
});
</script>

<template>
    <h1> My pokedex </h1>

    <input type="text" v-model="search" placeholder="Search for a pokemon" />

    <ul>
        <li v-for="(pokemon, index) in pokemonStore.filteredList" :key="`poke-${index}`">
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
