<template>
  <h1 class="d-flex justify-content-center align-items-center header">
    Math Quiz
  </h1>
  <h3 class="d-flex justify-content-center align-items-center">
    Try to go as far as you can!
  </h3>
  <div class="textWrapper">
    <h3>Questions answered:{{ questionCount + 1 }}</h3>
    <h1 class="d-flex justify-content-center align-items-center">
      {{ questions[questionCount].question }}
    </h1>
    <div>
      <h3>Questions correct: {{ correctQuestionsCount }}</h3>
      <h3>Questions wrong: {{ wrongQuestionsCount }}</h3>
    </div>
  </div>
  <div class="questionWrapper">
    <div
      v-for="answer in questions[questionCount].answers"
      @click="answerClick(answer)"
      class="d-flex justify-content-center align-items-center answerWrapper"
    >
      {{ answer }}
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
const questionCount = ref(0);
const correctQuestionsCount = ref(0);
const wrongQuestionsCount = ref(0);
const questions = ref<
  {
    question: string;
    correctAnswer: number;
    answers: number[];
  }[]
>([]);

function answerClick(answer: number) {
  if (answer === questions.value[questionCount.value].correctAnswer) {
    questionCount.value++;
    correctQuestionsCount.value++;
    generateQuestion();
  } else {
    wrongQuestionsCount.value++;
  }
}

function placeCorrectAnswerPosition() {
  questions.value[questionCount.value].answers.splice(
    Math.floor(Math.random() * 4),
    1,
    questions.value[questionCount.value].correctAnswer
  );
}
generateQuestion();
function generateQuestion() {
  const questionStructure = Math.random();
  const numberOne = Math.ceil(Math.random() * 100);
  const numberTwo = Math.ceil(Math.random() * 100);
  const numberThree = Math.ceil(Math.random() * 100);
  if (questionStructure <= 25) {
    questions.value.push({
      question: `${numberOne} + ${numberTwo} * ${numberThree}`,
      answers: [
        numberOne * (numberTwo - numberOne) * numberThree,
        numberOne * (numberTwo + numberThree),
        numberOne - numberTwo * numberThree,
        numberOne * (numberTwo - numberThree),
      ],
      correctAnswer: numberOne + numberTwo * numberThree,
    });
    questions.value[
      questionCount.value
    ].question = `${numberOne} + ${numberTwo} * ${numberThree}`;
  } else if (questionStructure <= 50 && questionStructure >= 25) {
    questions.value.push({
      question: `${numberOne} * (${numberTwo} + ${numberThree})`,
      answers: [
        numberOne - numberTwo * numberThree,
        numberOne + numberTwo * numberThree,
        numberOne + numberTwo ** 2 - numberThree,
        numberOne * (numberTwo - numberThree),
      ],
      correctAnswer: numberOne * (numberTwo + numberThree),
    });
  } else if (questionStructure <= 75 && questionStructure >= 50) {
    questions.value.push({
      question: `${numberOne} - ${numberTwo} * ${numberThree}`,
      answers: [
        numberOne * (numberTwo + numberThree),
        numberOne * (numberTwo - numberThree),
        numberOne + numberTwo * numberThree,
        numberTwo * numberThree,
      ],
      correctAnswer: numberOne - numberTwo * numberThree,
    });
  } else {
    questions.value.push({
      question: `${numberOne} * (${numberTwo} - ${numberThree})`,
      answers: [
        numberOne - numberTwo * numberThree,
        numberTwo - numberThree * numberOne + 3,
        numberOne + numberTwo * numberThree,
        numberOne * (numberTwo + numberThree),
      ],
      correctAnswer: numberOne * (numberTwo - numberThree),
    });
  }
  placeCorrectAnswerPosition();
}
</script>
<style scoped lang="scss">
.questionWrapper {
  display: grid;
  margin-inline: auto;
  margin-bottom: 25%;
  grid-template-columns: repeat(2, 1fr);
  width: 70vw;
  height: 40vh;
  color: black;
  gap: 3px;
}
.answerWrapper {
  background-color: rgba(128, 128, 128, 0.4);
  font-size: 36px;
  font-weight: bold;
}

h1,
h3 {
  color: black;
  font-weight: bold;
}

.header {
  padding-top: 15%;
}

.textWrapper {
  display: flex;
  justify-content: space-evenly;
}
</style>
