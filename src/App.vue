<template>
  <PageHeader img_url="/assets/logo.png" />
  <p v-if="error">An error occurred connecting to the server.</p>
  <div v-else>
    <CurrentQuestion msg="" :question="question" />
    <CurrentAnswers
      msg=""
      :answers="answers"
      :getQuestion="getQuestion"
      :answerQuestion="answerQuestion"
    />
    <p>{{ score }}</p>
    <button @click="getQuestion">Next Question</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import PageHeader from "./components/PageHeader.vue";
import CurrentQuestion from "./components/CurrentQuestion.vue";
import CurrentAnswers from "./components/CurrentAnswers.vue";
import { fetchRandomQuestion } from "@/Services/questionService";

interface Answer {
  text: string;
  correct: boolean;
}

interface Data {
  id: number;
  question: string;
  answer: string;
  incorrect_answer_1: string;
  incorrect_answer_2: string;
  difficulty: number;
  topic: string;
}

export default defineComponent({
  name: "App",
  components: { PageHeader, CurrentQuestion, CurrentAnswers },
  data() {
    return {
      answers: [] as Answer[],
      score: 0,
      questionAttempted: false,
      error: false,
      data: {
        id: "",
        question: "",
        answer: "",
        incorrect_answer_1: "",
        incorrect_answer_2: "",
        difficulty: 0,
        topic: "",
        error: false,
      },
    };
  },
  methods: {
    async getQuestion() {
      this.questionAttempted = false;
      this.data = await fetchRandomQuestion();
      if (this.data.error) {
        this.error = true;
      }
      this.answers = [
        { text: this.data.answer, correct: true },
        { text: this.data.incorrect_answer_1, correct: false },
        { text: this.data.incorrect_answer_2, correct: false },
      ].sort(() => Math.random() - 0.5);
    },
    answerQuestion(answer: Answer) {
      // increments the score if answered correctly first time
      if (answer.correct && !this.questionAttempted) {
        this.score++;
      }
      this.questionAttempted = true;
    },
  },
  computed: {
    question(): string {
      return this.data.question;
    },
  },
  mounted() {
    this.getQuestion();
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #f5f5f5;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 1.3em;
}

body {
  background-color: #252525;
}
</style>
