<template>
  <main class="columns is-gapless is-multiline">
    <div class="column is-one-quarter">
      <BarraLateral />
    </div>
    <div class="column is-three-quarter">
      <FormularioTask @aoSalvarTarefa="salvarTarefa" />
      <div class="lista">
        <TarefaTask
          v-for="(tarefa, index) in tarefas"
          :key="index"
          :tarefa="tarefa"
        />
        <BoxItem v-if="listaEstaVazia">
          Você não está muito produtivo hoje :(
        </BoxItem>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import BarraLateral from './components/BarraLateral.vue'
import FormularioTask from './components/Formulario.vue'
import TarefaTask from './components/TarefaTask.vue'
import ITarefa from './interfaces/ITarefa'
import BoxItem from './components/BoxItem.vue'

export default defineComponent({
  name: 'App',
  components: {
    BarraLateral,
    FormularioTask,
    TarefaTask,
    BoxItem,
  },
  data() {
    return {
      tarefas: [] as ITarefa[],
    }
  },
  computed: {
    listaEstaVazia(): boolean {
      return this.tarefas.length === 0
    },
  },
  methods: {
    salvarTarefa(tarefa: ITarefa) {
      this.tarefas.push(tarefa)
    },
  },
})
</script>

<style>
.lista {
  padding: 1.25rem;
}
</style>
