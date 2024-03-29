<template>
  <FormularioTask @aoSalvarTarefa="salvarTarefa" />
  <div class="lista">
    <BoxItem v-if="listaEstaVazia">
      Você não está muito produtivo hoje :(
    </BoxItem>
    <div class="field">
      <p class="control has-icons-left">
        <input
          class="input"
          type="text"
          placeholder="Digite para filtrar"
          v-model="filtro"
        />
        <span class="icon is-small is-left">
          <i class="fas fa-search"></i>
        </span>
      </p>
    </div>
    <TarefaTask
      v-for="tarefa in tarefas"
      :key="tarefa.id"
      :tarefa="tarefa"
      @aoTarefaClicada="selecionarTarefa"
    />
    <div
      class="modal"
      :class="{ 'is-active': tarefaSelecionada }"
      v-if="tarefaSelecionada"
    >
      <div class="modal-background"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Editando uma tarefa</p>
          <button
            @click="fecharModal"
            class="delete"
            aria-label="close"
          ></button>
        </header>
        <section class="modal-card-body">
          <div class="field">
            <label for="descricaoDaTarefa" class="label">Descrição</label>
            <input
              type="text"
              class="input"
              v-model="tarefaSelecionada.descricao"
              id="descricaoDaTarefa"
            />
          </div>
        </section>
        <footer class="modal-card-foot">
          <button @click="alterarTarefa" class="button is-success">
            Salvar alterações
          </button>
          <button @click="fecharModal" class="button">Cancelar</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import debounce from 'lodash.debounce'
import { computed, defineComponent, ref, watch } from 'vue'
import { useStore } from '@/store'
import FormularioTask from '../components/Formulario.vue'
import TarefaTask from '../components/TarefaTask.vue'
import ITarefa from '../interfaces/ITarefa'
import BoxItem from '../components/BoxItem.vue'
import {
  OBTER_TAREFAS,
  CADASTRAR_TAREFA,
  OBTER_PROJETOS,
  ALTERAR_TAREFA,
} from '@/store/tipo-acoes'

export default defineComponent({
  name: 'App',
  components: {
    FormularioTask,
    TarefaTask,
    BoxItem,
  },
  data() {
    return {
      tarefaSelecionada: null as ITarefa | null,
    }
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
    selecionarTarefa(tarefa: ITarefa) {
      this.tarefaSelecionada = tarefa
    },
    fecharModal() {
      this.tarefaSelecionada = null
    },
    alterarTarefa() {
      this.store
        .dispatch(ALTERAR_TAREFA, this.tarefaSelecionada)
        .then(() => this.fecharModal())
    },
  },
  setup() {
    const store = useStore()
    store.dispatch(OBTER_TAREFAS)
    store.dispatch(OBTER_PROJETOS)

    const filtro = ref('')

    // watchEffect(() => {
    //   store.dispatch(OBTER_TAREFAS, filtro.value)
    // })

    const search = debounce((filtro: string) => {
      store.dispatch(OBTER_TAREFAS, filtro)
    }, 500)

    watch(filtro, (value) => {
      search(value)
    })

    return {
      tarefas: computed(() => store.state.tarefa.tarefas),
      store,
      filtro,
    }
  },
})
</script>
