<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">{{currentQuestion.question}}</template>

      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']"
        >{{answer}}</b-list-group-item>
      </b-list-group>
      <div class="btn-wrap">
        <b-button variant="primary" href="#">Submit</b-button>
        <b-button variant="success" href="#" @click="next">Next</b-button>
      </div>
    </b-jumbotron>
  </div>
</template>


<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    next: Function,
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: [],
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
    },
  },
};
</script>

<style>
.btn {
  margin: 0 5px;
}
.btn-wrap {
  margin: 20px auto 0;
  width: fit-content;
}
.list-group-item {
  cursor: pointer;
}
/* .list-group-item:hover {
  background: #eee;
} */
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
