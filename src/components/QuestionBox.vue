<template>
  <div>
    <b-jumbotron>
      <template #lead>
        <span v-html="questionShown"></span>
      </template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          button
          :key="index"
          @click="selectAnswer(index)"
          :variant="selectedIndex === index ? 'info' : ''"
        >
          <span v-html="answer"></span>
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button variant="success" @click="nextQuestion">
        Next question
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  watch: {
    currentQuestion: {
      immediate: true, // to apply this after mounted
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
      },
    },
    // currentQuestion() {
    //   this.selectedIndex = null;
    //   this.shuffleAnswers();
    // },
    // Use this ^^^ along with the mounted function to shuffle at the beginning
  },
  computed: {
    questionShown() {
      return this.currentQuestion.question;
    },
    answers() {
      const answersArray = [...this.currentQuestion.incorrect_answers];
      answersArray.push(this.currentQuestion.correct_answer);
      return answersArray;
    },
    correctAnswer() {
      return this.currentQuestion.correct_answer;
    },
  },
  methods: {
    nextQuestion() {
      this.$emit("nextQuestion");
      this.answered = false;
    },
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      const answersArray = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];

      this.shuffledAnswers = _.shuffle(answersArray);

      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      if (this.selectedIndex) this.increment(isCorrect);
    },
  },
  // mounted() {
  //   this.shuffleAnswers();
  // },
};
</script>
<style scoped>
.list-group {
  margin-bottom: 20px;
}
.btn {
  margin: 0 5px;
}
</style>



