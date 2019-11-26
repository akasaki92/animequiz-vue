<template>
    <div id="app">
        <Header :numCorrect="numCorrect" :numTotal="numTotal" :totalQuestion="totalQuestion" :numQuestion="numQuestion"/>
        <b-container class="by-example-row">
            <b-row>
                <b-col sm="6" class="container">
                    <QuestionBox
                        v-if="questions.length"
                        :currentQuestion="questions[index]"
                        :next="next"
                        :increment="increment"
                        :numQuestion="numQuestion"
                        :totalQuestion="totalQuestion"
                    />
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
    name: "app",
    components: {
        Header,
        QuestionBox
    },
    data: function() {
        return {
            api:
                "https://opentdb.com/api.php?amount=10&category=31&difficulty=easy&type=multiple",
            questions: [],
            index: 0,
            numCorrect: 0,
            numTotal: 0,
            totalQuestion: 10,
            numQuestion: 1
        };
    },
    methods: {
        next: function() {
            if (this.numQuestion <= this.totalQuestion) {
                this.index++;
                this.numQuestion++;
            }
        },
        increment: function(isCorrect) {
            if (isCorrect) {
                this.numCorrect++;
            }
            this.numTotal++;
        }
    },
    mounted: function() {
        fetch(this.api, {
            method: "get"
        })
            .then(response => {
                return response.json();
            })
            .then(jsonData => {
                this.questions = jsonData.results;
            });
    }
};
</script>

<style>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>
