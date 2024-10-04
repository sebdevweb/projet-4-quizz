<template>
  <h2>Recap</h2>
  <p><strong>Votre score : </strong>{{ score }}/{{ quiz.questions.length }}</p>
  <p><strong>Vos réponses : </strong><span>{{ answers }}</span></p>
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
  p {
    span {
      font-style: italic;
      color: grey;
    }
  }
</style>