<script setup>
import { RouterLink, RouterView } from 'vue-router'
import { ref, watch } from 'vue'

//Template syntax
const mainTitle = 'Vue Cert Practice Space'
const dynamicId = ref("dynamicId")

//Watchers
const question = ref('')
const answer = ref('Questions usually contain a questions mark. ;)')
const loading = ref(false)

watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach API: ' + error
    } finally {
      loading.value = false
      console.log('Question changed:', oldQuestion, '->', newQuestion)
    }
  } else {
    answer.value = 'Questions usually contain a questions mark. ;)'
  }
})
</script>

<template>
  <div :id="dynamicId" class="vue-cert-practice-space">
    <h1>{{ mainTitle }}</h1>
    <p>A very basic site to practice the fundementals of Vue3</p>
    <div class="watchers">
      <h2>Watchers</h2>
      <p>
        Ask a yes/no question:
        <input type="text" v-model="question" :disable="loading"/>
      </p>
      <p>{{ answer }}</p>
    </div>
  </div>
</template>

<style scoped>
.vue-cert-practice-space {
  display: grid;
  align-content: flex-start;
  gap: 1rem;
  font-family: "Inter", serif;
  min-height: 100vh;
  background-color: #333;
  color: #fff;
  padding: 2rem;
}

.watchers {
  display: grid;
  gap: 0.5rem;
}
</style>
