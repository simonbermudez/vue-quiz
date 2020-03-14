<template>
    <div class="question-box-container">
        <b-jumbotron>

            <template v-slot:lead>
                <h6 v-html="currentQuestion.question"></h6>
            </template>

            <hr class="my-4">
            <b-list-group>
                <b-list-group-item 
                v-for="(answer, index) in answers" 
                :key="index" 
                v-html="answer" 
                @click.prevent="selectAnswer(answer)"
                :class="[
                    !answered && selectedAnswer === answer ? 'selected' : 
                    answered && selectedAnswer === currentQuestion.correct_answer ? 'correct' : ''
                ]"
                >
                </b-list-group-item>
            </b-list-group>

            <b-button 
            variant="primary"
            @click="submitAnswer"
            :disabled="selectedAnswer === null || answered"
            >
                Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
    },
    data() {
        return {
            selectedAnswer: null,
            answered: false
        }
    },
    watch: {
        currentQuestion() {
            this.selectedAnswer = null
            this.answered = false;
        }
    },
    methods: {
        selectAnswer(answer) {
            this.selectedAnswer = answer
        },
        submitAnswer() {
            let isCorrect = this.selectedAnswer === this.currentQuestion.correct_answer
            if(isCorrect) {
                alert("Correct!")
            }
            this.answered = true;
            this.increment(isCorrect)
        }
    },
    computed: {
        answers() {
            return _.shuffle([...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer])
        }
    }
}
</script>