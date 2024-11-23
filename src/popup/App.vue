<template>
	<div>
		<h2>Dados Carregados</h2>
		<DataFetcher :dataType="dataType" />

		<!-- Botões para gerar dados -->
		<button @click="generateRandomData('names')">Gerar Nomes</button>
		<button @click="generateRandomData('address')">Gerar Endereço</button>
		<button @click="generateRandomData('mails')">Gerar E-mail</button>
		<button @click="generateRandomData('phones')">Gerar Telefone</button>
	</div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import DataFetcher from '../components/Fetcher2.vue';

export default defineComponent({
	components: {
		DataFetcher,
	},

	setup() {
		const dataType = ref<string>('names');

		const generateRandomData = (type: string) => {
			if (dataType.value === type) {
				dataType.value = '';
				setTimeout(() => {
					dataType.value = type;
				}, 10);
			} else {
				dataType.value = type;
			}
		};

		return {
			dataType,
			generateRandomData,
		};
	},
});
</script>