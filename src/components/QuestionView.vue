<template>
	<div v-if="q != undefined">
		<div class="question">
			<h1>Question {{ questionIndex }}/{{ numberOfQuestions }}</h1>
			<h2> {{ q.question }}</h2>
		</div>
		<div class="responses">
			<Response v-for="answer of q.answers" :answer="answer" @click="emitanswer(answer)" />
		</div>
	</div>
</template>

<script setup lang="ts">
import { defineComponent } from 'vue'
import type { Question } from '../assets/ts/question'
import Response from './Response.vue'
</script>

<script lang="ts">

export default defineComponent({
	name: 'Question',

	emits: ['answer'],

	props: {
		q: Object as () => Question,
		numberOfQuestions: Number,
		questionIndex: Number
	},
	methods: {
		emitanswer(answer: string) {
			this.$emit('answer', answer)
		}
	}

})

</script>
<style scoped>
.responses {
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: space-around;
	flex-wrap: wrap;
	height: 10vh;
	width: 80vw;
}
</style>