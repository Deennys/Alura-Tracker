<template>
    <button class="button" @click="clicado" :disabled="cronometroRodando">
        <span class="icon">
            <i :class=icone></i>
        </span>
        <span>{{ valor }}</span>
    </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'Botao',
    emits: ['clicado'],
    props: {
        icone: {
            type: String
        },
        valor: {
            type: String
        },
        cronometroRodando: {
            type: Boolean
        }
    },
    methods: {
        clicado():void {
            this.$emit('clicado');
        }
    }
}) 
</script>