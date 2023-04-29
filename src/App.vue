<script setup>
import { ref, computed } from "vue";
const questions = ref([
  {
    question: "What is Vue.js?",
    answer: 0,
    options: ["A frontend framework", "A backend framework", "A library"],
    selected: null,
  },
  {
    question: "What is JavaScript?",
    answer: 2,
    options: [
      "Java but with Script",
      "A hosting website",
      "A programming language",
    ],
    selected: null,
  },
  {
    question: "What is Visual Studio Code?",
    answer: 1,
    options: [
      "A programming language",
      "An IDE",
      "A frontend framework",
      "A programming forum",
    ],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) value++;
  });
  return value;
});

const getCurrentQuesition = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1)
    currentQuestion.value++;
  else quizCompleted.value = true;
};
</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>
    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuesition.question }}</span>
        <span class="score">Score: {{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuesition.options"
          :key="index"
          :class="`option ${
            getCurrentQuesition.selected == index
              ? index == getCurrentQuesition.answer
                ? 'correct'
                : 'wrong'
              : ''
          } ${
            getCurrentQuesition.selected != null &&
            index != getCurrentQuesition.selected
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuesition.index"
            :value="index"
            v-model="getCurrentQuesition.selected"
            :disabled="getCurrentQuesition.selected"
            @change="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="NextQuestion" :disabled="!getCurrentQuesition.selected">
        {{
          getCurrentQuesition.index == questions.length - 1
            ? "Finish"
            : getCurrentQuesition.selected == null
            ? "Select an option"
            : "Next question"
        }}
      </button>
    </section>
    <section v-else>
      <h2>You have finished the quiz!</h2>
      <p>Score: {{ score }}/{{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
}

body {
  background-color: #271c36;
  color: #fff;
}

.app {
  display: flex;
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
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quiz-info .score {
  color: #fff;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #2d213f;
}

.option .correct {
  background-color: #2cce7d;
}

.option .wrong {
  background-color: #ff5a6f;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option .disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;

  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25;
  border-radius: 0.5rem;
  display: block;
  margin: 0 auto;
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
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align: center;
}
</style>
