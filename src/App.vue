<template>
  <div id="app">
    <div class="column is-offset-one-quarter is-half" id="poke">
      <h4 class="is-size-4">Pokedex</h4>
      <hr />
      <input
        class="input is-rounded"
        type="text"
        name=""
        id=""
        placeholder="Buscar pokemon "
        v-model="busca"
      />
      <button
        @click="buscar"
        class="button is-fullwidth is-success"
        id="buscaBtn"
      >
        Buscar
      </button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokedex :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokedex from "./components/Pokedex";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log(res);
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    Pokedex,
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.match(this.busca.toLocaleLowerCase())
        );
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

#buscaBtn {
  margin-top: 2%;
}
</style>
