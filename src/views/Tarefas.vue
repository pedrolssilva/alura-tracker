<template>
  <FormularioTask @aoSalvarTarefa="salvarTarefa" />
  <div class="lista">
    <BoxItem v-if="listaEstaVazia">
      Você não está muito produtivo hoje :(
    </BoxItem>
    <TarefaTask v-for="tarefa in tarefas" :key="tarefa.id" :tarefa="tarefa" />
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'
import { useStore } from '@/store'
import FormularioTask from '../components/Formulario.vue'
import TarefaTask from '../components/TarefaTask.vue'
import ITarefa from '../interfaces/ITarefa'
import BoxItem from '../components/BoxItem.vue'
import { ADICIONA_TAREFA } from '@/store/tipo-mutacoes'

export default defineComponent({
  name: 'App',
  components: {
    FormularioTask,
    TarefaTask,
    BoxItem,
  },
  computed: {
    listaEstaVazia(): boolean {
      return this.tarefas.length === 0
    },
  },
  methods: {
    salvarTarefa(tarefa: ITarefa) {
      this.store.commit(ADICIONA_TAREFA, tarefa)
    },
  },
  setup() {
    const store = useStore()
    return {
      projetos: computed(() => store.state.projetos),
      tarefas: computed(() => store.state.tarefas),
      store,
    }
  },
})
</script>
