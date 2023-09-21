<template>
  <FormularioTask @aoSalvarTarefa="salvarTarefa" />
  <div class="lista">
    <BoxItem v-if="listaEstaVazia">
      Você não está muito produtivo hoje :(
    </BoxItem>
    <TarefaTask
      v-for="(tarefa, index) in tarefas"
      :key="index"
      :tarefa="tarefa"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import FormularioTask from '../components/Formulario.vue'
import TarefaTask from '../components/TarefaTask.vue'
import ITarefa from '../interfaces/ITarefa'
import BoxItem from '../components/BoxItem.vue'

export default defineComponent({
  name: 'App',
  components: {
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
