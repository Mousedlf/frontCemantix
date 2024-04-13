<script >

import axios from "axios";
import {Word} from "@/Interface/word.js";

const url = "http://127.0.0.1:8000/embedding"
let similarity = {}


export default {
  data() {
    return {
      message: 'Trouve le mot secret',
      prompts: [],
    }
  },

  methods: {

    async fetch() {
      try {
        await axios.post(url, {prompt: this.prompt})
            .then((response) => {
              console.log(response.data.similarity)
              similarity =  response.data
              similarity = similarity['similarity']
              console.log(similarity)
              return similarity
            });

      } catch (error) {
        console.log("marche pas");
      }
    },

    sendPrompt() {

      similarity = this.fetch()

     // console.log(this.fetch)
     console.log(this.prompts)

      const word = new Word()
      word.value = this.prompt
      word.similarity = similarity

//

      if(this.prompts.includes(this.prompt) === false ){
        this.prompts.push(word)
        this.prompt = ''
      }
      this.prompt = ''
    }

    //si similarity = 100 alors "WIN"
  },

  computed: {
    // futur filtre pour ordre
  }

}

</script>

<template>

  <p>{{ message}}</p>
  <form @submit.prevent="sendPrompt(prompt)">
    <input v-model="prompt" type="text" name="" id="" placeholder="un mot">
    <button type="submit" class="btn btn-warning">OK</button>
  </form>

  <ul class="list-group mt-4 col-6">
    <li v-for="prompt in prompts"  class="list-group-item d-flex justify-content-between align-items-center">
      {{ prompt.value }}
      <span class="" >{{ prompt.similarity }}</span>
    </li>
  </ul>




</template>

<style scoped>

</style>