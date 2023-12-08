<template>
	<div class="game">
		<h1>Quizz</h1>
		<div v-if="!quizzStarted">
			<button @click="quizzStarted = true">Start</button>
		</div>
		<div v-else>
			<div v-if="!explanation">
				<QuestionView :q="questions[questionIndex]" @answer="verifAnswer($event)"
					:number-of-questions="questions.length" :question-index="questionIndex" />
				<h1>Score: {{ score }}</h1>
			</div>
			<div v-else>
				<h1>Correct answer: {{ questions[questionIndex - 1].correctAnswer }}</h1>
				<h1>explanation:</h1>
				<h2>{{ questions[questionIndex - 1].explanation }}</h2>
				<h3>Sources: {{ questions[questionIndex - 1].source }}</h3>
				<button @click="explanation = false">Next</button>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">
import { defineComponent } from 'vue'
import type { Question } from '../assets/ts/question'
import QuestionView from './QuestionView.vue'
</script>
<script lang="ts">

export default defineComponent({
	name: 'Quizz',

	data() {
		return {
			questions: [] as Question[],
			questionIndex: 0,
			score: 0,
			quizzStarted: false,
			explanation: false
		}
	},

	methods: {
		importQuestions() {
			// import questions from './questions.json'

			fetch("/questions.json")
				.then(response => response.json())
				.then((data: Question[]) => {
					this.questions = data
					console.log(this.questions)
				})
				.catch((error) => {
					console.error("Error:", error);
				});
		},
		verifAnswer(answer: string) {
			console.log(answer)
			console.log(this.questions[this.questionIndex].correctAnswer)
			console.log(answer === this.questions[this.questionIndex].correctAnswer)
			if (answer === this.questions[this.questionIndex].correctAnswer) {
				this.score++
			}
			this.questionIndex++
			this.explanation = true
		}
	},

	created() {
		this.importQuestions()
	},

})

</script>

<style scoped>
* {
	/* text border */
	-webkit-text-stroke: 1px black;
}

.game {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	background-color: rgba(0, 121, 10, 0.75);
	padding: 1em;
	border-radius: 1em;
}
</style>