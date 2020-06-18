<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || isAnswered">Submit</b-button>
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import lodash from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      isAnswered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.isAnswered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = this.selectedIndex === this.correctIndex;
      this.isAnswered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = lodash.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    answerClass(index) {
      if (!this.isAnswered && this.selectedIndex === index) {
        return 'selected';
      }
      if (this.isAnswered && index === this.correctIndex) {
        return 'correct';
      }
      if (this.isAnswered && index === this.selectedIndex && index !== this.correctIndex) {
        return 'incorrect';
      }
      return '';
    }
  }
}
</script>

<style scoped>
  .list-group {
    margin-bottom: 1em;
  }

  .list-group-item:hover {
    background-color: #eee;
    cursor: pointer;
  }

  .btn {
    margin: 0 0.25em
  }

  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: lightcoral;
  }

</style>