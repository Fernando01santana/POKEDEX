<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokedex.png" alt="" />
      <hr />
      <input
        class="input is-primary"
        type="text"
        placeholder="Search pokemon"
        v-model="busca"
      />
      <button class="button is-link is-light is-fullwidth mt-2" @click="buscar">
        search
      </button>
      <div class="column">
        <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
          <pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";
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
      .then((data) => {
        this.pokemons = data.data.results;
        this.filteredPokemons = data.data.results;
        console.log(this.pokemons);
      })
      .catch((err) => {
        console.lof(err);
      });
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca == "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(
          (pokemon) => pokemon.name == this.busca
        );
      }
    },
  },
  components: {
    Pokemon,
  },
  computed: {
    // resultadoBusca: function () {
    //   if (this.busca == "" || this.busca == " ") {
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
    //   }
    // },
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
.mt-2 {
  margin-top: 1em;
}
</style>
