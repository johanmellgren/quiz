<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        <span v-html="currentQuestion.question"></span>
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="[selectedIndex === index ? 'selected' : '']">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null
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
    currentQuestion() {
      this.selectedIndex = null;
      this.shuffleAnswers();
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      
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