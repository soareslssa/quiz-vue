<template>
  <div>
    
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />

    <template v-if="this.question">
      
       <h1 v-html="this.question"/>

      <template v-for="(answer, index) in this.answers" :key="index">
        <input 
        v-model="this.choosen_answer"
        :disabled="this.answerSubmited"
        type="radio" 
        name="options" 
        :value="answer">
        <label v-html="answer"/><br>
      </template>
      
      <section class="result" v-if="this.answerSubmited">
        <h4 
        v-if="this.choosen_answer == this.correctAnswer"
        v-html="'&#9989; You got it right! The answer ' +  this.correctAnswer  + ' is correct'"></h4>

        <h4 v-else v-html="'&#10060; You got it wrong! The answer ' + this.correctAnswer + ' is correct'"></h4>
        
        <button @click="getNewQuestion()" class="send">Next Question</button>
      </section>

      <button @click="submitAnswer()" v-if="!this.answerSubmited" class="send">Send</button>
    </template>
  
 
  </div>
</template>

<script>

import ScoreBoard from '@/components/ScoreBoard.vue'

export default {
  name: 'App',
  components: {
    ScoreBoard
  },
  data () {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      choosen_answer: undefined,
      answerSubmited: false,
      winCount: 0,
      loseCount: 0
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
    this.getNewQuestion()
  },
  methods: {
    submitAnswer() {
      if(!this.choosen_answer){
        alert('Pick one of the options');
        return;
      }

      this.answerSubmited = true;

      if(this.choosen_answer == this.correctAnswer){
        this.winCount++;
      } else {
        this.loseCount++;
      }
    },
    getNewQuestion() {

      this.question = undefined;
      this.choosen_answer = undefined;
      this.answerSubmited = false;

      this.axios
      .get('https://opentdb.com/api.php?amount=1&category=11&type=multiple')
      .then((response) => {
          this.question = response.data.results[0].question
          this.incorrectAnswers = response.data.results[0].incorrect_answers
          this.correctAnswer = response.data.results[0].correct_answer
      })
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
