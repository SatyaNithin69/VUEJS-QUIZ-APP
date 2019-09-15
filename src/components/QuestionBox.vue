<template>
  <div class="container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question }}</template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in answers"
          :key="index"
          @click.prevent="selectedAnswer(index)"
          :class="[
          !answered && selectedIndex === index ? 'selected': 
            answered && correctIndex === index ? 'correct' :
           answered && selectedIndex === index && correctIndex !== index ? 'incorrect': '' 
          ]"
        >{{ answer }}</b-list-group-item>
      </b-list-group>
      <b-button
        variant="primary"
        @click="submitAnswers"
        :disabled="selectedIndex === null || answered"
      >Submit</b-button>
      <b-button variant="success" @click="nextQuestion">Next</b-button>
    </b-jumbotron>
  </div>
</template>
<script>
export default {
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      answered: false
    };
  },
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    increment: Function
  },
  watch: {
    currentQuestion() {
      this.selectedIndex = null;
      this.answered = false;
    }
  },
  methods: {
    selectedAnswer: function(index) {
      this.selectedIndex = index;
    },
    submitAnswers: function() {
      let isCorrect = false;
      this.correctIndex = this.answers.indexOf(
        this.currentQuestion.correct_answer
      );
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    }
  },
  computed: {
    answers: function() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  }
};
</script>
<style scoped>
.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}
.selected {
  background: lightblue;
}
.correct {
  background: lightgreen;
}
.incorrect {
  background: red;
}
</style>
