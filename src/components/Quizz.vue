<template>
	<div class="game">
		<h1>Quizz</h1>
		<h3>Voici un quizz sur la thématique de l'écologie !</h3>
		<div v-if="!quizzStarted">
			<button @click="quizzStarted = true">Commencer !</button>
		</div>
		<div v-else>
			<div v-if="explanation">
				<h1 v-if="bonneReponse" class="good">Bonne réponse !!!</h1>
				<h1 v-else class="wrong">Mauvaise réponse ...</h1>
				<h1>Expliquation:</h1>
				<h2>{{ questions[questionIndex - 1].explanation }}</h2>
				<h1>Possible solution au problème:</h1>
				<h2>{{ questions[questionIndex - 1].solution }}</h2>
				<a :href="questions[questionIndex - 1].source" target="_blank">
					<h3>Sources: {{ questions[questionIndex - 1].source }}</h3>
				</a>
				<button @click="explanation = false">Suivant</button>
			</div>
			<div v-else-if="!finished">
				<QuestionView :q="questions[questionIndex]" @answer="verifAnswer($event)"
					:number-of-questions="questions.length" :question-index="questionIndex" />
				<h1>Score: {{ score }}</h1>
			</div>
			<div v-else>
				<h1>Vous avez fini le quizz ! Nous esperons que vous avez appris des choses et que vous avez
					pris consience si ce n'etais pas le cas de l'etat dans lequel nous mettons la planete</h1>
				<h1>Score: {{ score }}/{{ questions.length }}</h1>
				<h1>Vous avez eu {{ score / questions.length * 100 }}% de bonnes réponses</h1>
				<h2>Vous pouvez recommencer le quizz en rafraichissant la page</h2>
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
			explanation: false,
			bonneReponse: false,
			finished: false
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
				this.bonneReponse = true
			}
			else {
				this.bonneReponse = false
			}
			this.questionIndex++
			this.explanation = true
			if (this.questionIndex === this.questions.length) {
				this.finished = true
			}
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
	-webkit-text-stroke: 0.5px black;
}

p {
	text-align: center;
	font-size: 1.5em;
}

a {
	text-decoration: none;
	color: white;
}

a:hover {
	color: rgb(0, 2, 128);
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

.wrong {
	color: red;
}

.good {
	color: rgb(0, 255, 0);
}
</style>