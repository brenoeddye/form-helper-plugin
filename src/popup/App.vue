<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
	name: 'App',
	setup() {
		const handleClick = () => {
			alert('Botão clicado no Vue com TypeScript!');
		};

		return {
			handleClick,
		};
	},
});
</script>

<template>
	<div>
		<h1>Olá, Vue com TypeScript!</h1>
		<button @click="handleClick">Clique-me</button>
	</div>
</template>

<style scoped>
h1 {
	color: #42b983;
}
</style>