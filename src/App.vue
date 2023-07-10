<script setup>
import {ref, computed} from 'vue'

const questions = ref([ 
  {
    question: 'What is 2 + 2?',
    answer: 0,
    options: [
      '4',
      '22',
      '0',
      '2' 
    ],
    selected: null
  },
  {
    question: 'What is 4 * 2?',
    answer: 2,
    options: [
      '4',
      '22',
      '8',
      '2' 
    ],
    selected: null
  }

])

const quizCompleted = ref(false)
const currentQuestion = ref(0)

const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if(q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted.value = true
  }
}

const RestartQuiz = () => {
  quizCompleted.value = false
  currentQuestion.value = 0
  questions.value.map(q => {
    q.selected = null
  })
}

</script>

<template>
  <main class="app">
    <h1>Quiz App</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }}</span> 
      </div>
        <div class="options">
          <label v-for="(option, index) in getCurrentQuestion.options" :key="index" :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct'
                : 'incorrect'
              : ''
              
          } ${
            getCurrentQuestion.selected != null && index != getCurrentQuestion.selected 
              ? 'disabled'
              : ''
          }`">
            <input 
              type="radio" 
              :name="getCurrentQuestion.index" 
              :value="index" 
              v-model = "getCurrentQuestion.selected" 
              :disabled="getCurrentQuestion.selected" 
              @change="SetAnswer">
            <span>{{ option }}</span>
          </label>
        </div>
      <button @click="NextQuestion" :disabled = !getCurrentQuestion.selected>
        {{ 
          getCurrentQuestion.index == questions.length - 1 
            ? 'Finish' 
            : getCurrentQuestion.selected == null
              ? 'Select an option' 
              : 'Next question'
        }}
      </button>
    </section>
    <section v-else>
      <h2>DONE</h2>
      <p>Your score is {{ score }}/{{ questions.length }}</p>
      <button @click="RestartQuiz">Try again</button>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'montserrat', sans-serif;
}

body {
  background-color: #271c36;
  color: #FFF;
}

.app {
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz {
  width: 100%;
  background-color: #382a4b;
  padding: 1rem;

}

.quiz-info {

  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #8F8F8F;
  font-size: 1.25rem;
}

.quiz-info .score {
  color: #FFF;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  display: block;
  padding: 1rem;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  width: 100px;
}

.option:hover {
  background-color: #2d213f;
}

.option.correct {
  background-color: #2cce7d;
}

.option.incorrect {
  background-color: #ff5a5f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  cursor: pointer;

  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button:disabled {
  opacity: 0.5;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p {
  color: #8F8F8F;
  font-size: 1.25rem;
  text-align: center;
}

</style>
