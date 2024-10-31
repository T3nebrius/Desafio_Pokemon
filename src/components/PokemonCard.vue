<template>
    <div class="pokemon-card">
      <img
        :src="pokemon.image"
        :style="{ filter: pokemon.discovered ? 'none' : 'blur(2px) grayscale(100%)' }"
        alt="Pokemon"
      />
      <div v-if="!pokemon.discovered">
        <input
          type="text"
          v-model="userGuess"
          placeholder=""
          @keyup.enter="checkGuess"
        />
        <button @click="checkGuess">Descubrir</button>
      </div>
      <div v-else>
        <p class="pokemon-name">{{ pokemon.name }}</p>
      </div>
    </div>
  </template>
  
  <script>
export default {
  props: {
    pokemon: {
        type: Object,
        required: true
    },
        index: {
        type: Number,
        required: true
    }
  },
  data() {
    return {
      isDiscovered: false,
      userGuess: ""
    };
  },
  methods: {
    checkGuess() {
    let writed = this.userGuess.toString().toLowerCase().trim();
    let named = this.pokemon.name.toString().toLowerCase().trim();
    
    if (writed === named) { 
        this.isDiscovered = true;
        this.$emit('pokemonDiscovered', this.index); 
    } else {
        alert("Nombre incorrecto, intenta de nuevo.");
        this.isDiscovered = false;
    }
    }
  }
};

  </script>
  
  <style scoped>
  .pokemon-card {
    width: 150px;
    text-align: center;
    border: 1px solid #ddd;
    border-radius: 10px;
    padding: 10px;
    background-color: #f9f9f9;
  }
  
  .pokemon-card img {
    width: 100px;
    height: 100px;
  }
  
  input {
    margin-top: 10px;
    width: 100%;
    padding: 5px;
    border-radius: 5px;
    border: 1px solid #ddd;
  }
  
  button {
    margin-top: 5px;
    padding: 5px 10px;
    color: grey;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  .pokemon-name {
    font-weight: bold;
    color: green;
    margin-top: 10px;
  }
  </style>
  