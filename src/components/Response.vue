<template>
	<div class="response">
		<h1>{{ answer }}</h1>
	</div>
</template>

<script setup lang="ts">

</script>
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
	name: 'Response',

	props: {
		answer: String
	}

})

</script>

<style scoped>
.response {
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: center;
	padding: 1em;
	width: 20vw;
	border-radius: 1em;
	background-color: rgba(56, 56, 56, 0.5);
	transition: background-color 300ms;
	-webkit-text-stroke: 2px black;
}

.response:hover {
	background-color: rgba(56, 56, 56, 0.8);
}

h1 {
	margin: 0
}
</style>