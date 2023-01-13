<template>
    <div class="box formulario">
        <div class="columns">
            <div class="column is-5" role="form" aria-label="Formulario para criação de uma nova tabela">
                <input type="text" class="input" placeholder="Qual tarefa você deseja iniciar?" v-model="descricao">
            </div>
            <div class="column is-3">
                <div class="select">
                    <select v-model="idProjeto">
                        <option value="">Selecione o projeto</option>
                        <option :value="projeto.id" v-for="projeto in projetos" :key="projeto.id">
                            {{ projeto.nome }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="column">
                <Temporizador @aoTemporizadorFinalizado="finalizarTarefa" />
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue';
import Temporizador from './Temporizador.vue';
import { useStore } from 'vuex';

import { key } from '@/store/index';
import { TipoNotificacao } from '@/interfaces/INotificacao';
import { NOTIFICAR } from '@/store/tipo-mutacoes';

export default defineComponent({
    name: 'Formulário',
    emits: ['aoSalvarTarefa'],
    components: {
        Temporizador
    },
    methods: {
        
    },
    setup (props, { emit }) {
        const store = useStore(key);

        const descricao = ref('');
        const idProjeto = ref('');
        const projetos = computed(() => store.state.projeto.projetos);

        const finalizarTarefa = (tempoDecorrido: number): void => {
            const projeto = projetos.value.find(p => p.id == idProjeto.value);
            console.log(projeto);
            if(!projeto) {
                store.commit(NOTIFICAR, {
                titulo: 'Ops!',
                texto: 'Selecione um projeto antes de finalizar a tarefa!',
                tipo: TipoNotificacao.FALHA
                });
                return;
            }
            emit('aoSalvarTarefa', {
                duracaoEmSegundos: tempoDecorrido,
                descricao: descricao.value,
                projeto: projetos.value.find(proj => proj.id == idProjeto.value)
            })
            descricao.value = '';
        }

        return {
            descricao,
            idProjeto,
            projetos,
            finalizarTarefa
        }
    }
});
</script>

<style>
.formulario {
  background-color: var(--bg-primario);
  color: var(--texto-primario);
}
</style>