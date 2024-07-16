<template>
  <main class="container">
    <section>
      <img src="../public/Pokemon-Logo.jpg" alt="logo-texto-pokemon" />
      <h1>¿Quién es ese Pokémon?</h1>
      <p><strong>Pokémones descubiertos: {{ pokemonCount }}</strong></p>
    </section>
    <section class="list-pokemon">
      <PokemonCard v-for="pokemon in pokemons" :key="pokemon.name" :pokemon="pokemon"
        @pokemon-descubierto="pokemonDescubierto" />
    </section>
  </main>
</template>

<script>
import axios from "axios";
import PokemonCard from "./components/PokemonCard.vue";

export default {
  name: "App",

  components: {
    PokemonCard,
  },

  data() {
    return {
      pokemons: [],
    };
  },

  computed: {
    pokemonCount() {
      return this.pokemons.filter(pokemon => pokemon.pokemonDescubiertoEstado).length;
    }
  },

  created() {
    this.fetchPokemons();
  },

  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=20");
        const results = response.data.results;

        const promises = results.map(async (pokemon) => {
          const response = await axios.get(pokemon.url);
          return response.data;
        });

        const pokemonDetails = await Promise.all(promises);
        
        pokemonDetails.forEach(pokemon => {
          pokemon.pokemonDescubiertoEstado = false;
        });
        this.pokemons = pokemonDetails;
      } catch (error) {
        console.error("Error fetching data:", error);
      }
    },

    pokemonDescubierto(pokemonName) {
      const pokemon = this.pokemons.find(pokemon => pokemon.name === pokemonName);
      if (pokemon) {
        pokemon.pokemonDescubiertoEstado = true;
        this.$forceUpdate(); 
        alert(`¡Has descubierto a ${pokemon.name}!`);
      } else {
        alert("¡Pokémon incorrecto!");
      }
    },
  },
};
</script>

<style scoped>
.container {
  text-align: center;
  margin-top: 20px;
}

.list-pokemon {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

img{
  width: 20rem;
  height: 10rem;
  object-fit: contain;
}
</style>
