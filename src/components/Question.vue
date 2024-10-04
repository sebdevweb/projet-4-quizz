<template>
  <div class="question">
    <h3>{{ question.question }}</h3>
    <ul>
      <li v-for="(choice, index) in randomChoices" :key="choice">
        <Answer 
          :id="`answer${index}`" 
          :disabled="hasAnswer" 
          :value="choice"
          @change="onAnswer"
          :correctAnswer="question.correct_answer"
          v-model="answer"
        />
      </li>
    </ul>
    <!-- Au click on passe le nom de l'événement 'answer' et la réponse choisie par l'utilisateur : answer -->
    <div class="result">
      <p>{{ answer }}</p>
      <!-- <button @click="emits('answer', answer)">Question suivante</button> -->
    </div>
  </div>
</template>


<script setup>
  import { shuffleArray } from '@/functions/array'
  import { ref, computed, watch, onMounted, onUnmounted } from 'vue'
  import Answer from './Answer.vue';
  const props = defineProps({
    question: Object
  })
  const emits = defineEmits(['answer'])
  const answer = ref(null)
  const hasAnswer = computed(() => answer.value !== null)

  // Appel la fonction shuffleArray définie dans functions/array.js pour l'affichage random des données contenues dans un tableau
  const randomChoices = computed(() => shuffleArray(props.question.choices))
  
  /* Méthode pour le bogue d'affichage lorsqu'une réponse est validé et que le bouton question suivante n'est pas disabled.
  L'appel de la fonction watch est nécessaire dans ce cas précis.
  L'autre solution est disponible dans le composant Quiz, en appelant le composant Question à chaque validation d'une réponse
  */
 
  // watch(() => props.question, () => {
  //   answer.value = null
  // })

  let timer

  const onAnswer = () => {
    // answer.value = e.currentTarget.value
    clearTimeout(timer)
    timer = setTimeout(() => {
      emits('answer', answer.value)
    }, 1_500)
  }


  onMounted(() => {
    timer = setTimeout(() => {
      answer.value = ''
      onAnswer()
    }, 4_000)
  })
  onUnmounted(() => {
    clearTimeout(timer)
  })
</script>


<style lang="scss">
  .question {
    padding-top: 2rem;
    .result {
      display: flex;
      align-items: center;
      justify-content: flex-end;
      gap: 24px;
      p {
        display: flex;
        margin: 0;
        font-style: italic;
        color: grey;
      }
      button {
        // margin-left: auto;
        display: block;
      }
    }
  }
</style>