<template>
  <div id="app">
    <Navbar
      :numCorrect = "numCorrect"
      :numTotal = "numTotal"
      :index ="index"
    ></Navbar>
    <QuestionBox
      v-if = "questions.length"
      :currentQuestion = "questions[index]"
      :next="next"
      :increment = "increment">
    </QuestionBox>
    <LastCard v-if="this.index === 10" 
              :numCorrect = "numCorrect"
              :numTotal = "numTotal"
    />
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue';
import QuestionBox from './components/QuestionBox.vue'
import LastCard from './components/LastCard.vue'

import Vue from 'vue';
import VueConfetti from 'vue-confetti';
Vue.use(VueConfetti);

export default {
  name: 'App',
  components: {
    Navbar,
    QuestionBox,
    LastCard,
  },
  data(){
    return{
      questions: [],
      index: 0,
      numTotal: 1,
      numCorrect:0,
    }
  },
  mounted: function(){
    fetch("https://opentdb.com/api.php?amount=10&category=11&type=multiple", {
      method: 'get'
    })
    .then((response) => {
      return response.json();
      
    })
    .then ((jsonData) => {
      this.questions = jsonData.results;
      //cleaning from encoded signs and encoded hispanic characters 
      function convert(str)
      {
        str = str.replace(/&ldquo;/g, "\"")
        str = str.replace(/&rdquo;/g, "\"")
        str = str.replace(/&amp;/g, "");
        str = str.replace(/&eacute;/g, "e");
        str = str.replace(/&aacute;/g, "a");
        str = str.replace(/&ntilde;/g, "n");
        str = str.replace(/&oacute;/g, "o");
        str = str.replace(/&gt;/g, "");
        str = str.replace( /&lt;/g,"\"");
        str = str.replace(/&quot;/g, "\"");
        str = str.replace(/&#039;/g, "'");
        return str;
      }
      for(let i = 0; i < this.questions.length; i++)
      {
        let q = this.questions[i];
        q.question = convert(q.question);
        for(let j = 0; j < q.incorrect_answers.length; j++)
        {
          q.incorrect_answers[j] = convert(q.incorrect_answers[j]) ;
        }
      }
      for(let c of this.questions){
        c.correct_answer= convert(c.correct_answer);
      }
    })
  },
  methods:{
    next(){
      if(this.index < 10){
        this.index++;
      }
      if(this.index === 10){
        this.$confetti.start();
        setTimeout(function(){
          this.$confetti.stop();
        },2000)
      }
      if(this.numTotal < 10 && this.index < 10){
        this.numTotal++;
      }
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect++
      }
    },
  }
}
</script>

<style lang="scss">
@import '@/assets/_shared.scss';
*{
  box-sizing: border-box;
  margin:0;
  padding:0;
}
#app {
  color: $secondary-color;
  height: 100vh;
}
body{
  height: 100vh;
  @include flexDirection(column);
  background: #D66D75;
  background: -webkit-linear-gradient(to right, #E29587, #D66D75);  
  background: linear-gradient(to right, #E29587, #D66D75); 
}
h1{
  font-family: $font-headline;
  font-size: 4rem;
}
a{
  text-decoration:none;
}
</style>
