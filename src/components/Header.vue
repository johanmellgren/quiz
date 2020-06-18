<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>My Fancy Quiz App</b-nav-item>
      <b-nav-item disabled>Counter: {{noOfCorrect}}/{{noOfAnswers}}</b-nav-item>
    </b-nav>    
  </div>
</template>

<script>
export default {
  props: {
    noOfCorrect: Number,
    noOfAnswers: Number
  }
}
</script>