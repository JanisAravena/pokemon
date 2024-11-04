<template>
    <div class="pokemon-card">
      <img :src="pokemonImage" :class="{ discovered: isDiscovered }" />
      <div v-if="!isDiscovered">
        <input v-model="guess" @keyup.enter="checkGuess" class="guess-input" />
        <button @click="checkGuess" class="guess-button">Descubrir</button>
      </div>
      <div v-else>
        <p>{{ pokemon.name }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['pokemon'],
    data() {
      return {
        guess: '',
        isDiscovered: false,
      };
    },
    computed: {
      pokemonImage() {
        return this.isDiscovered ? this.pokemon.image : this.pokemon.image + '?filter=blur';
      },
    },
    methods: {
      checkGuess() {
        if (this.guess.toLowerCase() === this.pokemon.name.toLowerCase()) {
          this.isDiscovered = true;
          this.$emit('discovered');
        } else {
          alert('Nombre incorrecto');
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .pokemon-card {
    padding: 5px;
    text-align: center;
    width: 160px;
    height: 200px;
  }
  .pokemon-card img.discovered {
    filter: none;
  }
  .pokemon-card img {
    filter: blur(3px) grayscale(100%); /*El filtro que oculta cada pokemon, recomendaba ponerle 5px pero lo baje a 3px*/
  } 

  .card-reveal {
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  .guess-input, .guess-button {
    width: 100%;
    box-sizing: border-box;
    margin: 5px 0;
  }
  </style>