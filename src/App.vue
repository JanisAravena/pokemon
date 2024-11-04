<template>
  <div id="app">
    <img src="https://static.vecteezy.com/system/resources/thumbnails/027/127/591/small_2x/pokemon-logo-pokemon-icon-transparent-free-png.png" class="pokemon-logo large" alt="Pokemon Logo" />    <h1>¿Quién es ese Pokémon?</h1>
    <p>Pokemones descubiertos: <span class="discovered-count">{{ discoveredCount }}</span></p>
    <div class="pokemon-list">
      <PokemonCard
        v-for="pokemon in pokemons"
        :key="pokemon.name"
        :pokemon="pokemon"
        @discovered="incrementDiscoveredCount"
      />
    </div>
  </div>
</template>

<script>
import PokemonCard from './components/PokemonCard.vue';

export default {
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  methods: {
    incrementDiscoveredCount() {
      this.discoveredCount++;
    },
  },
  created() {
    this.$axios.get('https://pokeapi.co/api/v2/pokemon?limit=20')
      .then(response => {
        this.pokemons = response.data.results.map(pokemon => ({
          name: pokemon.name,
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${pokemon.url.split('/')[6]}.png`,
        }));
      });
  },
};
</script>

<style scoped>
.pokemon-logo.large {
  width: 450px; 
  height: auto;
}
.bold-text {
  font-weight: bold;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
  font-family: 'Roboto', sans-serif;
}

.pokemon-logo {
  display: block;
  margin: 0 auto;
  max-width: 800px; 
  height: auto;
}

.pokemon-list {
  display: grid;
  grid-template-columns: repeat(4, 1fr); 
  gap: 10px;
  justify-items: center;
}

.pokemon-card {
  margin: 10px;
}

.discovered-count {
  color: #d4af37; 
}
</style>