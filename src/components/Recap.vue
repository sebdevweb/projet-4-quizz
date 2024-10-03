<template>
  <h2>Recap</h2>
  <p>Votre score : <strong>{{ score }} / {{ quiz.questions.length }}</strong></p>
  <p>{{ answers }}</p>
  <p><strong>{{ hasWon ? quiz.success_message : quiz.failure_message }}</strong></p>
</template>


<script setup>
  import { computed } from 'vue'


  const props = defineProps({
     quiz: Object,
     answers: Array
  })

  const score = computed(() => {
    return props.quiz.questions.reduce((accumulator, question, k) => {
      if (question.correct_answer === props.answers[k]) {
        return accumulator+1
      }
      return accumulator
    }, 0)
  })

  const hasWon = computed(() => score.value >= props.quiz.minimum_score)
</script>


<style lang="scss">

</style>