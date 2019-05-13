<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">{{ currentQuestion.question }}</template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click="setSelectedAnswer(index)"
          :class="getAnswerClass(index)"
        >{{ answer }}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex===null || answered"
      >Submit</b-button>
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
      answered: false,
      correctIndex: null
    };
  },
  computed: {
    answers() {
      return _.shuffle([
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer
      ]);
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.correctIndex = this.answers.indexOf(
          this.currentQuestion.correct_answer
        );
      }
    }
  },
  methods: {
    setSelectedAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      this.answered = true;
      if (this.selectedIndex === this.correctIndex) {
        return this.increment(true);
      }
      return this.increment(false);
    },
    getAnswerClass(index) {
      if (!this.answered && this.selectedIndex === index) {
        return "selected";
      }
      if (this.answered) {
        if (this.correctIndex === index) {
          return "correct";
        }
        if (this.selectedIndex === index && this.correctIndex !== index) {
          return "incorrect";
        }
      }
      return "";
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eeeeee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
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
