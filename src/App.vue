<template>
 
  <div class="searchDiv">
 
        <v-text-field class="searchBar" label="search" variant="outlined" v-model="search"></v-text-field>

        <v-btn icon>
          <img src="./assets/lupa.png" class="icon-img" alt="">
        </v-btn>

  </div>

  <div id="app" >
    
    <div v-for="(poke, index) in resultSearch" :key="poke.name" class="poke-item">
      <Pokemon :name="poke.name" :url="poke.url" :num="index" />
    </div>
  </div>  

</template>

<script>
import Pokemon from './components/Pokemon.vue'
import axios from "axios"
export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      search: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results
      console.log("pegou a lista de pokemons")
    })
  },
  components: {
    Pokemon
  },
  computed: {
    resultSearch: function() {
    if (this.search === '') {
      return this.pokemons;
    } else {
      const searchTerm = this.search.toLowerCase();
      return this.pokemons.filter(pokemon => 
        pokemon.name.toLowerCase().includes(searchTerm) ||
        pokemon.url.includes(searchTerm) // Verifica se a URL da imagem inclui o termo de pesquisa
      );
    }
  }
  }
}
</script>

<style>
  #app {
    display: flex;
    flex-wrap: wrap;
    box-sizing: border-box;
    justify-content: center;
    align-items: flex-start;
    flex-grow: 1;

  }

  .searchDiv {
    flex-basis: 100%;
    display: flex;
    padding: 20px;
    justify-content: center;
    align-items: center;
    padding-right: 40px;
    margin-left: 30%;
    margin-right: 30%;
    }

  .searchBar btn {
    padding-left: 20px;
  }

  .poke-item {
    display: flex;
    width: 300px; /* Ajuste conforme necessário */
    margin: 20px;
    padding: 20px;
    justify-content: center; /* Centraliza na vertical */
    padding-top: 30px;  
  }

  .searchBar {
    width: 200px;
    margin-top: 20px;
    height: 20%;
    margin-left: 20px;
  }

  
  .icon-img {
    max-width: 50%; /* Define a largura máxima da imagem como 100% */
    max-height: 50%; /* Define a altura máxima da imagem como 100% */
  }
  
</style>
