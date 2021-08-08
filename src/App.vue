<template>
  <h1>Выберите покемона</h1>

  <div class="container" id="app">
    <PokemonList
      :imageUrl="imageUrl"
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl"/>
    <PokemonItem 
    v-if="showItem"
    :pokemonUrl="pokemonUrl"
    :imageUrl="imageUrl"
    @closeItem="closeItem"/>
  </div>
</template>

<script>
import PokemonList from '@/components/PokemonList';
import PokemonItem from '@/components/PokemonItem';
export default {
  data: () => {
    return {
      imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      apiUrl: 'https://pokeapi.co/api/v2/pokemon?limit=20',
      pokemonUrl: '',
      showItem: false
    }
  },
  components: {
    PokemonList, 
    PokemonItem
  },
  methods: {
    setPokemonUrl(url){
      this.pokemonUrl = url;
      this.showItem = true;
    },
    closeItem(){
      this.pokemonUrl = "";
      this.showItem = false;
    }
  }
}
</script>



<style lang="sass">
h1
  font-family: Play
hr
  border: 2px solid 
  margin-top: 20px
#app 
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50


#nav 
  padding: 30px

  a 
    font-weight: bold
    color: #2c3e50

    &.router-link-exact-active 
      color: #42b983

.container 
  display: flex
  flex-direction: column
  padding: 10px
  width: calc( 100% - 30% )
  min-height: calc( 100vh - 40vh )
  font-size: 1rem
  font-weight: normal
  justify-content: center
  align-items: center
  margin: auto

</style>
