<template>
    <div class="question-box-container">
        
        <h2>{{ currentQuestion.question }}</h2>
        <div class="answer_container">
            <ul>
                <li 
                    v-for="(answer, index) in shuffledAnswers" 
                    :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class= 'answerColor(index)'>
                    {{ answer }}
                </li>
            </ul>
            <button class="btn_main"
                @click = "submitAnswer"
                :disabled = "selectedIndex === null || answered"
                v-show = "startGame === true">
                Submit answer
            </button>
            <button class="btn_main" 
                    @click="next"
                    v-show = "startGame === true">
                    Next question
            </button>
        </div>
    </div>
</template>

<script>
import _ from 'lodash';
import {eventBus} from '../main.js'; 

export default {
    props:{
        currentQuestion: Object,
        next: Function,
        increment: Function,
        totalSeconds: Number,
    },
    data(){
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            correctIndex: null,
            answered: false,
            startGame: false,
        }
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex = null;
                this.answered = false;
                this.shuffleAnswers();
            }
        }
    },
    methods:{
        selectAnswer(index){
           this.selectedIndex = index;
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false;
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true;
            this.increment(isCorrect);
        },
        answerColor(index){
            let answerColor= '';
            if(!this.answered && this.selectedIndex === index){
                answerColor = 'selected'
            }else if(this.answered && this.correctIndex === index){
                answerColor = 'correct'
            }else if(this.answered && this.correctIndex !== index){
                answerColor = "incorrect"
            }
            return answerColor;
        },
    },
    created(){
        eventBus.$on('start-quiz', () => {
            this.startGame = true;
        })
    }
    
   
}
</script>


<!-- STYLE ------------------------------------------->

<style lang="scss" scoped>
@import '@/assets/_shared.scss';
.answer_container{
    width: 500px;
}
ul{
    list-style-type: none;
    margin: 2rem auto;
    @include flexDirection(column);
    li{
    @extend %text-wrapper;
    width: 80%;
    padding: 1rem 0;
    margin-bottom: 1rem;
    font-family: $font-stack;
    transition: all 0.4s;
    }   
}
li:hover,
.selected{
    color: #EEE;
    box-shadow: 6px 6px 9px #D66D75 inset, -6px -6px 10px  #E29587 inset;
}
.correct{
    border: 2px solid #3fada8;
}
.incorrect{
    border: 2px solid red;
}
.question-box-container{
    margin-top: 3rem;
    z-index: 0;
    @include flexDirection(column);
}
h2{
    font-family: $font-stack;
    font-size: 1.5rem;
    width: 80%;
}
.btn_main{
    @extend %button-shared;
    background-color: $secondary-color;
    padding: 0.5rem 1rem;
    margin: 0.7rem;
    transition: all 0.4s ease-in-out;
    &:hover{
        transform: scale(1.2);
        outline: none;
    }
}
/*Media queries*/
@media (max-width: 600px){
    h2{
        margin-top: o; 
        font-size: 1rem;
    }
    .answer_container{
        width: 70%;
    }
    ul{
        margin: 1rem auto;
        li{
            padding: 0.5rem 0;
            margin-bottom: 0.5rem;
        }
    }
}
</style>