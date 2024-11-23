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
  name: 'DataFetcher',
  props: {
    dataType: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const data = ref<DataResponse | null>(null);
    const loading = ref<boolean>(true);
    const error = ref<string | null>(null);
    const generatedData = ref<string | null>(null);

    // Mapeia os tipos para os arquivos JSON locais importados
    const dataMapping: { [key: string]: any } = {
      names: names,
      address: address,
      mails: mails,
      phones: phones,
    };

    // Função para gerar um dado aleatório
    const generateRandomData = () => {
      if (data.value) {
        switch (props.dataType) {
          case 'names':
            if (data.value.firstName && data.value.secondName && data.value.lastName) {
              const firstName = data.value.firstName[Math.floor(Math.random() * data.value.firstName.length)];
              const secondName = data.value.secondName[Math.floor(Math.random() * data.value.secondName.length)];
              const lastName = data.value.lastName[Math.floor(Math.random() * data.value.lastName.length)];

              // Combina o nome completo
              generatedData.value = `${firstName} ${secondName} ${lastName}`;
            }
            break;

          case 'address':
            if (data.value) {
              const address = data.value[Math.floor(Math.random() * data.value.length)];
              generatedData.value = address;
            }
            break;

          case 'mail':
            if (data.value) {
              const mail = data.value[Math.floor(Math.random() * data.value.length)];
              generatedData.value = mail;
            }
            break;

          case 'phones':
            if (data.value) {
              const phone = data.value[Math.floor(Math.random() * data.value.length)];
              generatedData.value = phone;
            }
            break;
        }
      }
    };

    // Carrega os dados diretamente do arquivo JSON
    const loadData = (type: string) => {
      try {
        loading.value = true;
        error.value = null;

        // Verifica se o tipo de dado existe no mapeamento
        const jsonData = dataMapping[type];

        if (!jsonData) {
          throw new Error(`Tipo de dado não encontrado: ${type}`);
        }

        // Carrega os dados do JSON
        if (type === 'names') {
          data.value = {
            firstName: jsonData.firstName,
            secondName: jsonData.secondName,
            lastName: jsonData.lastName,
          };
        } else {
          data.value = jsonData;
        }

        // Gera o dado aleatório após carregar
        generateRandomData();
      } catch (err) {
        error.value = (err as Error).message;
      } finally {
        loading.value = false;
      }
    };

    onMounted(() => {
      loadData(props.dataType); // Carrega os dados ao montar o componente
    });

    watch(() => props.dataType, (newType) => {
      loadData(newType); // Carrega os dados novamente se o tipo mudar
    });

    // Render Function
    return () =>
      h('div', [
        loading.value
          ? h('div', 'Carregando...')
          : error.value
          ? h('div', `Erro: ${error.value}`)
          : h('div', [
              h('h3', 'Dado Gerado'),
              generatedData.value ? h('p', generatedData.value) : h('p', 'Clique para gerar um dado'),
            ]),
      ]);
  },
});
</script>