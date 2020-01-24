<template>
    <div class = "question-box-container">
        <b-jumbotron>

            <template slot="lead">
                {{ currentQuestion.question }}
            </template>

            <b-list-group>
                <b-list-group-item 
                v-for="(answer, index) in shuffled" 
                :key="index" 
                @click.prevent="selectAnswer(index)" 
                :class="[(answered == false && selectedIndex == index) ? 'selected' : (answered && correctIndex == index) ? 'correct' : (answered && correctIndex != index && selectedIndex == index ? 'wrong' : '')]" 
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <hr class="my-4">

            <b-button variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex == null || answered == true"
            >
                Submit
            </b-button>
            <b-button variant="success" 
                @click=next 
            >
                Next
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    data(){
        return {
            selectedIndex: null,
            shuffled: [],
            correctIndex: null,
            answered: false
        }
    },
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    watch:{
        currentQuestion:{
            immediate: true,
            handler(){
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
        },
        shuffleAnswers(){
            let answers = this.currentQuestion.incorrect_answers

            answers.push(this.currentQuestion.correct_answer)

            this.shuffled = _.shuffle(answers)
            // console.log(this.shuffled)

            this.correctIndex = this.shuffled.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false

            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true
            // console.log(this.currentQuestion.correct_answer, this.correctIndex)
            this.increment(isCorrect)
        }
    }
}
</script>

<style scoped>
    .list-group{
        margin: 10px;
    }

    .list-group-item:hover{
        background-color: bisque;
        cursor: pointer;
    }

    .btn{
        margin: 0 5px;
    }

    .selected{
        background-color: aqua;
    }

    .correct{
        background-color: greenyellow;
    }

    .wrong{
        background-color: tomato;
    }
</style>