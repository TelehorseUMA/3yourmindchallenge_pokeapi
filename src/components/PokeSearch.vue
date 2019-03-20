<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <label for="poke-name">Please enter name of Pokemon:</label>
    <input 
      v-model="searchstring"
      v-on:input="searchPokemon"  
      name="poke-name"
      type="text"
      >
    <br />
    <ul>
    <li v-show='errAPI'><small>{{ errAPI }}</small></li>
    <li v-show='errInput'><small>{{ errInput }}</small></li>
    </ul>
    <br />

    <AbilitiesList :abilities="this.abilities"></AbilitiesList>

  </div>
</template>

<script>
import axios from 'axios'
import AbilitiesList from './AbilitiesList'

export default {
  name: 'PokeSearch',
  data () {
    return {
      msg: 'Coding challenge for 3YOURMIND',
      pokeName: '',
      abilities: null,
      searchstring: '',
      errInput: '',
      errAPI: '',
    }
  },
  methods: {
    searchPokemon: function () {
      setTimeout(
        () => {
          if (this.searchstring.length >= 3) {
            this.errInput = ''
            axios
              .get(`https://pokeapi.co/api/v2/pokemon/${this.searchstring.toLowerCase()}`)
              .then(res => 
                  (this.abilities = res.data.abilities, this.pokeName = this.searchstring.toLowerCase(), this.errAPI='', localStorage.setItem(`${this.pokeName}`, `${this.abilities}` ))
              )
              .catch(err => (this.errAPI = `No Pokemon with that name found (${err})`))
          }
          else {
          this.errInput = 'Pokemon\'s name must be at least 3 characters long.'
          }
        }, 1500)
    },
    // checkLocalStorage: function () {

    // }
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
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
