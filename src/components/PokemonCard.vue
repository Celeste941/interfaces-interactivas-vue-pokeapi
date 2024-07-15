<template>
  <div class="pokemon-card">
    <img 
    :src="pokemon.sprites.other['dream_world'].front_default" 
    :style="{filter: pokemonDescubiertoEstado ? 'none' : 'blur(10px) grayscale(100%)',}" 
    alt="Imagen-del-pokémon" />
    <h1 v-if="pokemonDescubiertoEstado">{{ pokemon.name }}</h1>

    <form v-if="!pokemonDescubiertoEstado" @submit.prevent="descubrirPokemon">
      <input v-model="nombrePoke" type="text" placeholder="Escribe un nombre...." />
      <button type="submit">Descubrir</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",
  data() {
    return {
      nombrePoke: "",
      pokemonDescubiertoEstado: false,
      intentos: 0,
    };
  },

  props: {
    pokemon: Object,
  },

  methods: {
    descubrirPokemon() {
      // Comparar el nombre ingresado con el nombre del Pokémon
      if (this.nombrePoke === this.pokemon.name) {
        // Mostrar la imagen y el nombre del Pokémon
        this.pokemonDescubiertoEstado = true;
        // Incrementar el contador de aciertos
        this.$emit("pokemon-descubierto", this.pokemon.name);
      } else {
        // Mostrar alerta de error
        alert("¡Pokémon incorrecto!");
      }
    },
  },
};
</script>

<style scoped>

.pokemon-card {
  width: 15rem;
  border: 1px solid #ccc;
  margin: 10px;
  padding: 10px;
  text-align: center;
  background-color: #f0f0f0;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;

  /* Hover effect */
  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  }
}

img {
  width: 10rem;
  height: 10rem;
  /* border-radius: 50%; */
  /* object-fit: cover; */
  /* Ajusta la imagen al tamaño del contenedor */
  /* border: 2px solid #fff; */
  /* box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); */
}

h1{
  text-transform: uppercase;
}

form {
  margin-top: 10px;
}

/* Estilo para el input */
input[type="text"] {
  padding: 8px;
  margin-bottom: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

/* Estilo para el botón */
button[type="submit"] {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}
</style>
