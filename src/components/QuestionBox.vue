<template>
  <div>
    <b-jumbotron>
      <template #lead>
        <span v-html="currentQuestion"></span>
      </template>

      <hr class="my-4" />

      <p>List of answer</p>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#" @click="onClick"
        >Next question</b-button
      >
    </b-jumbotron>
  </div>
</template>
<script>
export default {
  props: {
    currentQuestion: String,
  },
  methods: {
    onClick() {
      this.$emit("nextQuestion");
    },
  },
};
</script>



