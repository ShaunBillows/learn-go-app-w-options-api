<template>
  <p>{{ msg }}</p>
  <ul class="answer-container">
    <li
      v-for="(answer, i) in answers"
      :key="i"
      class="answer"
      :class="{
        green: answer.correct && answer.clicked,
        red: !answer.correct && answer.clicked,
      }"
      @click="clickAnswer(answer)"
    >
      {{ answer.text }}
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface Answer {
  text: string;
  correct: boolean;
  clicked: boolean;
}

export default defineComponent({
  name: "CurrentAnswers",
  props: {
    msg: String,
    answers: Array,
    getQuestion: {
      type: Function,
      required: true,
    },
    answerQuestion: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    clickAnswer(answer: Answer) {
      this.answerQuestion(answer);
      answer["clicked"] = true;
    },
  },
});
</script>

<style scoped>
.answer-container {
  display: flex;
  text-decoration: none;
  flex-direction: row;
  gap: 2rem;
  list-style: none;
  max-width: 1100px;
  min-height: 300px;
}
.answer {
  padding: 2rem;
  border: 1px solid black;
  max-width: 350px;
  width: 100%;
  background-color: #bdbdbd;
  color: #252525;
  border-radius: 8px;
}
.red {
  background-color: #f44336;
}
.green {
  background-color: #4caf50;
}
</style>
