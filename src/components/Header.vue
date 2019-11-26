<template>
    <header>
        <div>
            <b-nav tabs>
                <b-nav-item disabled><b>Fancy Quizapp</b></b-nav-item>
                <b-nav-item disabled>Correct Answer : {{ numCorrect }}/{{ numTotal }}</b-nav-item>
                <b-nav-item disabled>Number Question : {{ numQuestion }}/{{ totalQuestion }}</b-nav-item>
            </b-nav>
        </div>
    </header>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal',
        'totalQuestion',
        'numQuestion'
    ]
}
</script>