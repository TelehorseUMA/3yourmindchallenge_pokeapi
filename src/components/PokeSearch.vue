<template>
  <div class="hello">
    <h1>{{ title }}</h1>
    <label for="poke-name">Please enter name of Pokemon:</label>
    <input 
      v-model="searchstring"
      v-on:input="searchPokemon"  
      name="poke-name"
      type="text"
      >
    <br />
    <small v-show='isLoading'>Loading...</small>
    <ul>
    <li v-show='errAPI'><small>{{ errAPI }}</small></li>
    <li v-show='errInput'><small>{{ errInput }}</small></li>
    </ul>
    <br />

    <AbilitiesList v-show="!errInput && !errAPI" :abilities="this.abilities"></AbilitiesList>

  </div>
</template>

<script>
import axios from 'axios'
import AbilitiesList from './AbilitiesList'

export default {
  name: 'PokeSearch',
  data () {
    return {
      title: 'Coding challenge for 3YOURMIND',
      pokeName: '',
      abilities: null,
      searchstring: '',
      errInput: '',
      errAPI: '',
      isLoading: false
    }
  },
  methods: {
    searchPokemon: function () {
      this.isLoading = true
      setTimeout(
        () => {
          if (this.searchstring.length >= 3) {
            this.isLoading = false
            this.errInput = ''
            axios
              .get(`https://pokeapi.co/api/v2/pokemon/${this.searchstring.toLowerCase()}`)
              .then(res => 
                  (this.abilities = res.data.abilities, 
                  this.pokeName = this.searchstring.toLowerCase(), 
                  this.errAPI='', 
                  localStorage.setItem(`${this.pokeName}`, `${this.abilities}` ))
              )
              .catch(err => (this.errAPI = `No Pokemon with that name found (${err})`))
          }
          else {
          this.isLoading = false
          this.errInput = 'Pokemon\'s name must be at least 3 characters long.'
          }
        }, 500)
    },
    checkLocalStorage: function () {
      let keysArray = Object.keys(localStorage)
      console.log(this.searchstring)
      if (keysArray.includes(this.searchstring)) {
        (this.abilities = localStorage[`${this.searchstring}`])
      } 
    }
  },
  components: {
    AbilitiesList
  }    
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
a {
  color: #42b983;
}
</style>
