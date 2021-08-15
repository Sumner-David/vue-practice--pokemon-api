<template>

  <div class="flex justify-center w-full">
    <input type="text" placeholder="Enter pokemon here" class="p-2 mt-10 border-2 border-blue-400" v-model="text">
  </div>

  <div class="flex flex-wrap justify-center p-4 mt-10">

    <div
      class="ml-4 text-2xl text-blue-500"
      v-for="(pokemon,index) in filteredPokemon"
      :key="index"
    >

    <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
      {{ pokemon.name }}
    </router-link>
    </div>
  </div>

  <div class="home">
    <h3>
      Hello World
    </h3>

  </div>
</template>

<script>
// @ is an alias to /src

import { reactive, toRefs, computed } from 'vue';

export default {
  name: 'Home',
  setup() {

    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: '',
      filteredPokemon:computed( () => updatePokemon())
    })

    function updatePokemon() {
      if(!state.text) {
        return []
      }

      return state.pokemons.filter( (pokemon) => {
        return pokemon.name.includes(state.text)
      })


    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
    .then(res => res.json())
    .then((data)=> {
      console.log(data);
      state.pokemons = data.results;

      state.urlIdLookup = data.results.reduce((accumulator, current, index) =>
        accumulator = { ...accumulator, [current.name]: index+1 }
      , {})

      // state.urlIdLookup = data.results.reduce((acc, cur, idx)=>
      //      acc = {...acc, [cur.name]:idx+1 }
      // ,{})
    })

    return { ...toRefs(state) }
  }
}
</script>
