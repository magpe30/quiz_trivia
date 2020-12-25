<template>
<div class="wrapper">
    <div class="lastcard_wrapper">
    <h1>Movie Quiz</h1>
    <h2>Well done you!</h2>
    <p>Your score is {{numCorrect}} / {{numTotal}}</p>
    <a href="/" class="btn">Again</a>
    </div>
</div>

</template>

<script>
export default{
    props: [ 'numCorrect', 'numTotal']
}
</script>

<style lang="scss" scoped>
@import '@/assets/_shared.scss';
.wrapper{
    margin: 0 auto;
}
.lastcard_wrapper{
    @extend %card-shared;
    h1{
        margin-top: 1.5rem;
    }
}
h2{
    font-size: 2rem;
    font-family: $font-stack;
    margin: 2.5rem 0;
}
p{
    margin: 2.5rem 0;
    font-family: $font-stack;
    font-weight: 900;
    font-size: 2rem;
}
.btn{
    @extend %button-shared;
    background-color: $secondary-color;
    box-shadow: 6px 6px 9px #110018, -6px -6px 10px   #410457;
    margin-top: 4rem;
    &:hover{
        box-shadow: inset 6px 6px 9px #110018, -6px -6px 10px   #410457;  
    }
}
/*Media queries*/
@media (max-width: 600px){
    .lastcard_wrapper{
        @include cardMobile;
        left: 10%;
        h1{
            font-size:1.5rem;
        }
        h2{
            font-size: 1rem;
        }
        p{
            font-size: 1rem;
        }
        .btn{
            width: 50%;
            margin: 0 4rem;
        }
    }

}
</style> 