<template>
  <div class="box formulario">
    <div class="columns">
      <div
        class="column is-5"
        role="form"
        aria-label="Formulário para criação de uma nova tarefa"
      >
        <input
          type="text"
          class="input"
          placeholder="Qual tarefa você deseja iniciar?"
          v-model="descricao"
        />
      </div>
      <div class="column is-3">
        <div class="select">
          <select v-model="idProjeto">
            <option value="">Selecione o projeto</option>
            <option
              :value="projeto.id"
              v-for="projeto in projetos"
              :key="projeto.id"
            >
              {{ projeto.nome }}
            </option>
          </select>
        </div>
      </div>
      <div class="column">
        <Temporizador @aoTemporizadorFinalizado="salvarTarefa" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue'
import Temporizador from './Temporizador.vue'
import { useStore } from 'vuex'
import { key } from '@/store'
import { TipoNotificacao } from '@/interfaces/INotificacao'
import useNotificador from '@/hooks/notificador'

export default defineComponent({
  name: 'FormularioTask',
  emits: ['aoSalvarTarefa'],
  components: {
    Temporizador,
  },
  methods: {},
  setup(props, { emit }) {
    const descricao = ref('')
    const idProjeto = ref('')
    const projetos = computed(() => store.state.projeto.projetos)

    const store = useStore(key)
    const { notificar } = useNotificador()

    const salvarTarefa = (tempoEmSegundos: number): void => {
      const projeto = projetos.value.find((p) => p.id == idProjeto.value)
      if (!projeto) {
        notificar(
          TipoNotificacao.FALHA,
          'Ops!',
          'Selecione um projeto antes de finalizar a tarefa!'
        )
        return
      }
      emit('aoSalvarTarefa', {
        duracaoEmSegundos: tempoEmSegundos,
        descricao: descricao.value,
        projeto: projetos.value.find((proj) => proj.id == idProjeto.value),
      })
      descricao.value = ''
    }

    return {
      descricao,
      idProjeto,
      projetos,
      salvarTarefa,
    }
  },
})
</script>

<style>
.formulario {
  color: var(--texto-primario);
  background-color: var(--bg-primario);
}
</style>
