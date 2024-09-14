<template>
    <div class="pokemon-card">
      <img :src="pokemon.image" :class="{ hidden: !discovered }" alt="Imagen del Pokémon" />
      <div v-if="!discovered">
        <input v-model="userInput" @keyup.enter.prevent="checkName" placeholder="¿Quién es este pokémon?" />
        <button @click="checkName">Descubrir</button>
      </div>
      <p v-else>{{ pokemon.name }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemon: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        userInput: '',
        discovered: false
      };
    },
    methods: {
      checkName() {
        if (this.userInput.trim().toLowerCase() === this.pokemon.name.toLowerCase()) {
          this.discovered = true;
          this.$emit('discovered');
        } else {
          alert('Nombre incorrecto, intenta de nuevo.');
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .pokemon-card {
    text-align: center;
    border: 1px solid #a14444;
    padding: 10px;
    border-radius: 10px;
    width: 150px;
    background-color: #e7e405;
    margin: auto;
  }
  
  img {
    width: 100px;
    height: 100px;
    transition: filter 0.5s ease;
  }
  
  .hidden {
    filter: brightness(0) invert(1);
  }
  
  input {
    margin-top: 10px;
    padding: 5px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }
  
  button {
    margin-top: 10px;
    padding: 5px 10px;
    border-radius: 5px;
    border: none;
    background-color: #007bff;
    color: white;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  p {
    margin-top: 10px;
    font-weight: bold;
    font-size: 18px;
  }
  </style>
  