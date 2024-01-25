<template>
  <FormularioTask @aoSalvarTarefa="salvarTarefa" />
  <div class="lista">
    <BoxItem v-if="listaEstaVazia">
      Você não está muito produtivo hoje :(
    </BoxItem>
    <TarefaTask v-for="tarefa in tarefas" :key="tarefa.id" :tarefa="tarefa" />
    <div class="modal">
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Modal title</p>
          <button class="delete" aria-label="close"></button>
        </header>
        <section class="modal-card-body">
          <!-- Content ... -->
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success">Save changes</button>
          <button class="button">Cancel</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'
import { useStore } from '@/store'
import FormularioTask from '../components/Formulario.vue'
import TarefaTask from '../components/TarefaTask.vue'
import ITarefa from '../interfaces/ITarefa'
import BoxItem from '../components/BoxItem.vue'
import {
  OBTER_TAREFAS,
  CADASTRAR_TAREFA,
  OBTER_PROJETOS,
} from '@/store/tipo-acoes'

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
      this.store.dispatch(CADASTRAR_TAREFA, tarefa)
    },
  },
  setup() {
    const store = useStore()
    store.dispatch(OBTER_TAREFAS)
    store.dispatch(OBTER_PROJETOS)
    return {
      tarefas: computed(() => store.state.tarefas),
      store,
    }
  },
})
</script>
