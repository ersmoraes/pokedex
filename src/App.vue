<template>
 <div class="container">
    <div class="linhas">        
      <div class="colunas">
        <input type="text" class="barra-pesquisa " placeholder="Pesquisar pokémon" v-model="search">
      </div>
  </div>
  <div class="linhas">
      <div class="pokedex-catalogo">
        <!-- início listagem dinâmica -->
        <div 
          v-for="pokemon in filtered_pokemons" 
          :key="pokemon.id"
          :class="`cartao-pokemon`">

          <div class="card">
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(pokemon)}.png`" :alt="pokemon.name">
              <div class="text">
                <h1>{{get_name(pokemon)}}</h1>
                <span>#{{get_id(pokemon)}}</span>
              </div>
          </div>
        </div>
        <!-- fim listagem dinâmica -->

      </div>
    </div>
 </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  components: {},
  data() {
    return {
      pokemons: [],
      search: ""
    }
  },

  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151").then((response) => {
      this.pokemons = response.data.results
    })
  },
  methods: {
    get_id(pokemon) {
      return Number(pokemon.url.split('/')[6])
    },
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1)
    },
  },
  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search)
      })
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-size: 16px;
}

body {
  background: #DCDCDC;
}

.container {
    width: 100%;
    margin-top: 20px;
    margin-left: 20px;
}

.linhas {
    display: flex;
    flex-wrap: wrap;
}
.colunas {
  flex: 1 0 0%
}

.barra-pesquisa {
    display: block;
    width: 95%;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: #212529;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    appearance: none;
    border-radius: 0.25rem;
    transition: border-color .15s ease-in-out,box-shadow .15s ease-in-out;
}

.pokedex-catalogo {
  display: flex;
  flex-wrap: wrap;
  height: 400px;
  width: 100%;
  margin: 0 8px;
}

.cartao-pokemon {
  background: white;
  position: relative;
  margin: 5px;
  width: 15%;
  height: 200px;
  border-radius: 5px;
  -webkit-box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
  -moz-box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
  box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
}

.card img {
  display: block;
  width: 75%;
  margin: 10px auto;
}

.text {
  display: flex;
  justify-content: space-between;
  margin-left: 10px;
  margin-right: 10px;
}
</style>