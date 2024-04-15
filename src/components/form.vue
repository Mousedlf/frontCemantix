<script >

import axios from "axios";
import {Word} from "@/Interface/word.js";

const url = "http://127.0.0.1:8000/embedding"
let similarity
let hasWon


export default {
  data() {
    return {
      message: 'Trouve le mot secret',
      prompts: [],
    }
  },

  methods: {

    async sendPrompt() {
      await axios.post(url, {prompt: this.prompt})
          .then((response) => {
            console.log("hello" + response.data.similarity)
            similarity = response.data.similarity
          });

      const word = new Word()
      word.value = this.prompt
      word.similarity = similarity

      if(this.prompts.includes(this.prompt) === false ){
        this.prompts.push(word)
        this.prompt = ''
      }

      this.prompts.sort((a,b)=> b.similarity - a.similarity )

      this.prompt = ''

      if(word.similarity === 100){
        this.hasWon = "Bravo"
      }
    }

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

  <h1>{{ hasWon }}</h1>

  <ul class="list-group mt-4 col-6">
    <li v-for="prompt in prompts"  class="list-group-item d-flex justify-content-between align-items-center">
      {{ prompt.value }}
      <span class="" >{{ prompt.similarity }}</span>
    </li>
  </ul>




</template>

<style scoped>

</style>