<template>
  <div class="app">
    <img class="logo" src="https://cdn.worldvectorlogo.com/logos/pokemon-23.svg">
    <h1 class="mainTitle">¿Quién es ese Pokemón?</h1>
    <p>Pokémones descubiertos:</p><p class="counter">{{ discoveredCount }}/20</p>
    <div class="pokemon-list">
      <PokemonCard v-for="(pokemon, index) in pokemons" :key="pokemon.name" :pokemon="pokemon" :index="index" @pokemonDiscovered="incrementDiscovered" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";

export default {
  components: { PokemonCard },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  methods: {
    getRandomOffset() {
      return Math.floor(Math.random() * 1018);
    },
    async fetchPokemons() {
      const offset = this.getRandomOffset();
      const path = `https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=20`;
      try {
        const response = await axios.get(path);
        const results = response.data.results;
        this.pokemons = await Promise.all(
          results.map(async (pokemon) => {
            const details = await axios.get(pokemon.url);
            return {
              name: pokemon.name,
              image: details.data.sprites.front_default,
              discovered: false,
            };
          })
        );
      } catch (error) {
        alert("Error en la consulta a la API: " + path);
      }
    },
    incrementDiscovered(index) {
      this.pokemons[index].discovered = true;
      this.discoveredCount++;
    },
  },
  created() {
    this.fetchPokemons();
  },
};
</script>

<style scoped>
.app {
  text-align: center;
  margin: 20px;
}

.pokemon-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.logo {
  width: 200px;
}

.counter {
  color: goldenrod;
}

.mainTitle {
  font-family: Arial, Helvetica, sans-serif;
}
</style>
