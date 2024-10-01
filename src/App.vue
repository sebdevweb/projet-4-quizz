<template>
  <div class="container">
    <div v-if="state === 'error'">
      <p>Impossible de charger le questionnaire</p>
    </div>
    <div :aria-busy="state === 'loading'">
      <Quiz :quiz="quiz" v-if="quiz" />
    </div>
  </div>
</template>


<script setup>
  import { onMounted, ref } from 'vue'
  import Quiz from './components/Quiz.vue'

  const quiz = ref(null)
  const state = ref('loading')

  onMounted(() => {
    fetch('/quiz.json')
      .then( response => {
        if (response.ok)  {
          return response.json()
        }
        throw new Error('Impossible de récupérer le json')
      })
      .then(data => {
        quiz.value = data
        state.value = 'idle'
      })
      .catch(error => {
        state.value = 'error'
      })
  })
</script>

<style lang="scss">
  body {
    margin: 0;
    padding: 0;
    font-family: sans-serif;
    .container {
      margin-top: 2rem;
    }
  }
</style>
