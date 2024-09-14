<!-- src/App.vue -->

<template>
  <div id="app">
    <h1>¿Quién es este Pokémon?</h1>
    <ul>
      <li v-for="pokemon in pokemonList" :key="pokemon.name">
        <PokemonCard :pokemon="pokemon" @discovered="incrementScore" />
      </li>
    </ul>
    <p>Puntuación: {{ score }}</p>
  </div>
</template>

<script>
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',
  components: {
    PokemonCard
  },
  data() {
    return {
      pokemonList: [],
      score: 0
    };
  },
  created() {
    this.fetchPokemonList();
  },
  methods: {
    async fetchPokemonList() {
      try {
        const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=20');
        const data = await response.json();
        this.pokemonList = await Promise.all(
          data.results.map(async pokemon => {
            const res = await fetch(pokemon.url);
            const details = await res.json();
            return {
              name: details.name,
              image: details.sprites.front_default
            };
          })
        );
      } catch (error) {
        console.error("Error al obtener la lista de Pokémon:", error);
      }
    },
    incrementScore() {
      this.score++;
    }
  }
};
</script>

<style>
h1 {
  font-size: 50px; color: brown; text-shadow: #918e8e 0.1em 0.1em 0.2em;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 10px;
}
</style>
