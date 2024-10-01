<template>
  <div class="question">
    <h3>{{ question.question }}</h3>
    <ul>
      <li v-for="(choice, index) in question.choices" :key="choice">
        <label :for="`answer${index}`">
          <input :id="`answer${index}`" type="radio" name="answer" v-model="answer" :value="choice">
          {{ choice }}
        </label>
      </li>
    </ul>
    {{ answer }}
    <!-- Au click on passe le nom de l'événement 'answer' et la réponse choisie par l'utilisateur : answer -->
    <button :disabled="!hasAnswer" @click="emits('answer', answer)">Question suivante</button>
  </div>
</template>


<script setup>
  import { ref, computed } from 'vue'
  const props = defineProps({
    question: Object
  })
  const emits = defineEmits(['answer'])
  const answer = ref(null)
  const hasAnswer = computed(() => answer.value !== null)
</script>


<style lang="scss">
  .question {
    padding-top: 2rem;
    button {
      margin-left: auto;
      display: block;
    }
  }
</style>