<script lang="ts">
import { defineComponent, ref, onMounted, watch, h } from 'vue';

import address from     '../data/address.json';
import mails from       '../data/mails.json';
import names from       '../data/names.json';
import phones from      '../data/phones.json';

interface DataResponse {
    [key: string]: any;
}

export default defineComponent({
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
                    names:      names,
                    address:    address,
                    mails:      mails,
                    phones:     phones,
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