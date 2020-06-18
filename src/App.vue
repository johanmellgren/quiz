<template>

  <div id="app">
    <form class="form-inline">
      <div class="form-group mx-sm-3 mb-2">
        <label for="category" class="sr-only">Email</label>
        <select class="form-control" id="exampleFormControlSelect1">
          <option v-for="(category, index) in categories" :key="index">
            {{ category.name }}
          </option>
        </select>
      </div>
      <div class="form-group mx-sm-3 mb-2">
        <label for="noOfQuestions" class="sr-only">No of Questions</label>
        <input type="password" class="form-control" id="noOfQuestions" placeholder="#">
      </div>
      <button type="button" class="btn btn-primary mb-2">Get Questions</button>
    </form>

    <Header 
      :noOfCorrect="noOfCorrect"
      :noOfAnswers="noOfAnswers"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import axios from 'axios';

export default {
  name: "app",
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      categories: [],
      noOfCorrect: 0,
      noOfAnswers: 0,
      noOfQuestions: 10
    }
  },
  methods: {
    next() {
      if (this.index < this.noOfQuestions - 1) {
        this.index++
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.noOfCorrect++;
      }
      this.noOfAnswers++;
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=24&type=multiple', {method: 'get'}).then(response => {
      return response.json();
    }).then(data => {
      this.questions = data.results;
      console.log(this.questions);
    });

    axios.get('https://opentdb.com/api_category.php').then(response => {
      this.categories = JSON.parse(JSON.stringify(response.data.trivia_categories));
      console.log(this.categories);
    })
    .catch(error => {
      console.log(error);
    })
    .finally(() => {
      console.log('ready');
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
