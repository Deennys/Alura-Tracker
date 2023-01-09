<template>
  <Formulario @aoSalvarTarefa="salvarTarefa" />
  <div class="lista">
      <Box v-if="listaEstaVazia">
          Você não está muito produtivo hoje :(
      </Box>
      <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa"/>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import Formulario from '../components/Formulario.vue';
import Tarefa from '../components/Tarefa.vue';
import Box from '../components/Box.vue';
import ITarefa from '../interfaces/ITarefa';
import { useStore } from '@/store';
import { OBTER_TAREFAS } from '@/store/tipo-acoes';

export default defineComponent({
  name: 'App',
  components: {
    Formulario,
    Tarefa,
    Box
  },
  data () {
    return {
      modoEscuroAtivo: false
    }
  },
  computed: {
    listaEstaVazia (): boolean {
      return this.tarefas.length === 0;
    }
  },
  methods: {
    salvarTarefa(tarefa: ITarefa): void {
      this.tarefas.unshift(tarefa);
    },
    trocarTema(modoEscuroAtivo: boolean) {
      this.modoEscuroAtivo = modoEscuroAtivo;
    }
  },
  setup () {
    const store = useStore();
    store.dispatch(OBTER_TAREFAS);
    return {
      tarefas: computed(() => store.state.tarefas),
      store
    }
  }
});
</script>

<style>
.lista{
  padding: 1.25rem;
}
main {
  --bg-primario: #fff;
  --texto-primerio: #000;
}
main.modo-escuro {
  --bg-primario: #2b2d42;
  --texto-primario: #ddd;
}
.conteudo {
  background-color: var(--bg-primario);
  color: var(--texto-primario);
}
</style>