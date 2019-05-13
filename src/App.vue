<template>
  <div id="app">
    <Header :cntCorrect="cntCorrect" :cntAnswered="cntAnswered"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[curIndex]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      curIndex: 0,
      cntCorrect: 0,
      cntAnswered: 0
    };
  },
  methods: {
    next() {
      this.curIndex += 1;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.cntCorrect += 1;
      }

      this.cntAnswered += 1;
    }
  },
  async mounted() {
    this.questions = await fetch(
      "https://opentdb.com/api.php?amount=10&category=27&type=multiple"
    )
      .then(r => r.json())
      .then(r => r.results);
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
