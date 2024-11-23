<template>
	<div>
		<h2>Dados Carregados</h2>
		<DataFetcher :dataType="dataType">
			<template #default="{ data, loading, error }">
				<!-- Verifica o estado de carregamento e erros -->
				<div v-if="loading">Carregando...</div>
				<div v-else-if="error">Erro: {{ error }}</div>
				<div v-else>
					<!-- Exibe os dados carregados -->
					<pre>{{ data }}</pre>
				</div>
			</template>
		</DataFetcher>

		<!-- Opções para alterar o tipo de dado carregado -->
		<button @click="dataType = 'names'">Carregar Nomes</button>
		<button @click="dataType = 'address'">Carregar Endereços</button>
		<button @click="dataType = 'mail'">Carregar E-mails</button>
		<button @click="dataType = 'phones'">Carregar Telefones</button>
	</div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import DataFetcher from './components/Fetcher.vue';

export default defineComponent({
	components: {
		DataFetcher,
	},

	setup() {
		const dataType = ref<string>('names');

		return {
			dataType,
		};
	},
});
</script>