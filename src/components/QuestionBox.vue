<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                <span v-html="currentQuestion.question"></span>
            </template>

            <hr class="my-4" />

            <b-list-group>
                <b-list-group-item
                    v-for="(answer, index) in shuffledAnswers"
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                >{{ answer }}</b-list-group-item>
            </b-list-group>

            <b-button
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >Submit</b-button>
            <b-button @click="next" :disabled="numQuestion === totalQuestion || !answered" variant="success">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from "lodash";

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        numQuestion: Number,
        totalQuestion: Number
    },
    data: function() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        };
    },
    computed: {
        answers: function() {
            let answers = [...this.currentQuestion.incorrect_answers];
            answers.push(this.currentQuestion.correct_answer);
            return answers;
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler: function() {
                this.selectedIndex = null;
                this.answered = false;
                this.shuffleAnswers();
            }
        }
    },
    methods: {
        answerClass: function(index) {
            let answerClass = "";

            if (!this.answered && this.selectedIndex === index) {
                answerClass = "selected";
            } else if (this.answered && this.correctIndex === index) {
                answerClass = "correct";
            } else if (
                this.answered &&
                this.selectedIndex === index &&
                this.correctIndex !== index
            ) {
                answerClass = "incorrect";
            }

            return answerClass;
        },
        selectAnswer: function(index) {
            this.selectedIndex = index;
        },
        submitAnswer: function() {
            let isCorrect = false;

            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
        },
        shuffleAnswers: function() {
            let answers = [
                ...this.currentQuestion.incorrect_answers,
                this.currentQuestion.correct_answer
            ];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(
                this.currentQuestion.correct_answer
            );
        }
    },
    mounted: function() {
        this.shuffleAnswers();
        console.log(this.currentQuestion);
    }
};
</script>

<style scoped>
.list-group-item:hover {
    background-color: lightblue;
    cursor: pointer;
}
.selected {
    background-color: lightblue;
}
.correct {
    background-color: lightgreen;
}
.incorrect {
    background-color: lightcoral;
}
.btn {
    margin: 15px 5px;
}
</style>