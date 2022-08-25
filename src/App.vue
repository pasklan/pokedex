<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokemon-logo.png" alt="" />
      <hr />
      <h4 class="title is-size-4">Pokedex</h4>
      <input
        type="text"
        name="searchByName"
        id="searchByName"
        placeholder="Buscar pelo nome completo"
        class="input is-warning is-large"
        v-model="search"
      />
      <!-- <button class="button is-warning" id="btnSearch">Pesquisar</button> -->
      <div v-for="poke in resultadoBusca" :key="poke.url">
        <PokemonsList :name="poke.name" :url="poke.url" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonsList from "./components/PokemonsList";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      search: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then(res => {
        this.pokemons = res.data.results;
        console.log("Lista Recuperada");
      })
      .catch(err => {
        console.log(err);
      });
  },
  components: {
    PokemonsList,
  },
  computed: {
    resultadoBusca: function () {
      if (this.search === "" || this.search === " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.search);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#btnSearch {
  margin-top: 2%;
}
</style>
