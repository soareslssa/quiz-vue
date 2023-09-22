<template>
  <div>

    <template v-if="this.question">
       <h1 v-html="this.question"/>

      <template v-for="(answer, index) in this.answers" :key="index">
        <input 
        v-model="this.choosen_answer"
        type="radio" 
        name="options" 
        :value="answer">
        <label v-html="answer"/><br>
      </template>

      <button @click="submitAnswer()" class="send">Send</button>
    </template>
  
 
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      choosen_answer: undefined
    }
  },
  computed: {
   answers() {
    let answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
    answers.splice(Math.round(Math.random() * answers.length), 0 ,this.correctAnswer);
    return answers;
   }
  },
  created () {
    this.axios
    .get('https://opentdb.com/api.php?amount=1&category=11&type=multiple')
    .then((response) => {
        this.question = response.data.results[0].question
        this.incorrectAnswers = response.data.results[0].incorrect_answers
        this.correctAnswer = response.data.results[0].correct_answer
    })
  },
  methods: {
    submitAnswer() {
      if(!this.choosen_answer){
        alert('Pick one of the options');
        return;
      }

      if(this.choosen_answer == this.correctAnswer){
        alert('You got it right!');
      } else {
        alert('You got ir wrong!')
      }
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto ;
  max-width: 960px;

  input[type=radio] {
    margin: 12px 4px;
  }

  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: white;
    background-color: forestgreen;
    border: 1px solid forestgreen;
    cursor: pointer;
  }
}
</style>
