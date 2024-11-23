<script lang="ts">
import { defineComponent, ref, onMounted, watch, h } from 'vue';

interface DataResponse {
    [key: string]: any;
}

export default defineComponent({
    name: 'DataFetcher',
    props: {
        dataType: {
            type: String,
            required: true,
        },
    },
    setup(props, { slots }) {
        const data = ref<DataResponse | null>(null);
        const loading = ref<boolean>(true);
        const error = ref<string | null>(null);

        const loadData = async (type: string) => {
            try {
                loading.value = true;
                error.value = null;

                const dataMapping: { [key: string]: string } = {
                    names:      '../../data/names.json',
                    address:    '../../data/address.json',
                    mails:      '../../data/mails.json',
                    phones:     '../../data/phones.json',
                };

                const jsonPath = dataMapping[type];

                if (!jsonPath) {
                    throw new Error(`Tipo de dado não encontrado: ${type}`);
                }

                const response = await fetch(jsonPath);
                if (!response.ok) {
                    throw new Error('Erro ao buscar dados do servidor');
                }

                data.value = await response.json();
            } catch (err) {
                error.value = (err as Error).message;
            } finally {
                loading.value = false;
            }
        };

        onMounted(() => {
            loadData(props.dataType);
        });

        watch(() => props.dataType, (newType) => {
            loadData(newType);
        });

        // Função de renderização
        return () =>
            h('div', [
                slots.default
                    ? slots.default({
                        data: data.value,
                        loading: loading.value,
                        error: error.value,
                    })
                    : null,
            ]);
    },
});
</script>