<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal"/>
    <b-container class="bv-example-row">
      <b-row sm="6" offset="3">
        <QuestionBox
          v-if="questions.length"
          :currentQuestion="questions[index]"
          :nextQuestion="next"
          :increment="increment"
        />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    };
  },
  methods: {
    next: function() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted: function() {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=12&difficulty=easy&type=multiple"
    )
      .then(response => {
        return response.json();
      })
      .then(response => {
        this.questions = response.results;
      });
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
