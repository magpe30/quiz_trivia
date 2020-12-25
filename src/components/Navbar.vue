<template>
    <div>
        <nav>
            <h1>Movie quiz</h1>
            <div class="stats_wrapper">
                <button class="btn_main" @click="handleStart"
                        v-show="totalSeconds === (2*60)" >Start Quiz</button>
                <div class="text_wrapper">
                    <p>Timer {{ displayMinutes}}:{{ displaySeconds }}</p>  
                </div>
                <div class="text_wrapper">
                    <p>Score counter: {{ numCorrect }}/{{ numTotal }}</p>
                </div>
            </div>
            <!-- The Card that pops out at the end when a user finishes the quiz  -->
            <div class="pop_out"
                v-if="totalSeconds === 0">
                <h1>Movie Quiz</h1>
                <p>Ohh noo! Your time run out!</p>
                <a href="/" class="btn_second">Try again</a>
            </div>
        </nav>
    </div>
</template>

<script>
import {eventBus} from '../main.js';

export default{
    props: [
        'numTotal', 'numCorrect', 'startGame', 'index'
    ],
    data(){
        return{
           totalSeconds: 2 * 60,
        }
    },
    computed:{
        displayMinutes(){
            const minutes =  Math.floor(this.totalSeconds/60)
            return this.formatTime(minutes)
        },
        displaySeconds(){
            const seconds = this.totalSeconds % 60
            return this.formatTime(seconds)
        },
        
    },
    methods:{
        formatTime(time){
            if(time < 10){
                return '0' + time
            }
            return time.toString()
        },
        handleStart(){
            eventBus.$emit('start-quiz');
            const timer = setInterval(() =>{
               if(this.totalSeconds > 0){
                    this.totalSeconds -=1;   
               }
               if(this.totalSeconds === 0 || this.index === 10){
                   clearInterval(timer);
               }
            },1000)
        }
    },
}
</script>

<style lang="scss" scoped >
@import '@/assets/_shared.scss';

nav{
    z-index:0;
    @include flexDirection(column);
}
.stats_wrapper{
    @include flexDirection(row);
    margin-top: 1.5rem;
    a{
      color: $secondary-color;  
    }
}
.text_wrapper{
    @extend %text-wrapper;
    padding: 1rem 2rem;
    margin: 0 1rem;
}
p{
    font-family: $font-stack;
    font-weight: 900;
    top:50%;
}
.pop_out{
  @extend %card-shared;
  h1{
    margin: 3rem 0;
  }
  p{
    margin: 4rem 0;
  }
}
.btn_second{
    @extend %button-shared;
    background-color: $secondary-color;
    box-shadow: 6px 6px 9px #110018, -6px -6px 10px   #410457;
    &:hover{
        box-shadow: inset 6px 6px 9px #110018, -6px -6px 10px   #410457;  
    }
}
.btn_main{
   @extend %button-shared;
   background-color: $secondary-color; 
}
/* Media queries */
@media (max-width: 600px){
    h1{
        font-size: 2.5rem;
    }
    .stats_wrapper{
        @include flexDirection(column);
    }
    .text_wrapper{
        padding: 0.5rem 1rem;
        margin-top: 1rem;
    }
    .pop_out{
        @include cardMobile;

        h1{
            font-size:1.5rem;
            margin: 2rem 0;
        }
        h2,p{
            font-size: 1rem;
            margin: 2rem 0;
        }
        .btn_second{
            margin: 4rem 4.3rem;
            width: 50%;
        }
    }
}
</style>

