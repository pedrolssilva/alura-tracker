<template>
  <BoxItem>
    <div class="columns">
      <div class="column is-4">
        {{ tarefa.descricao || 'Tarefa sem descrição' }}
      </div>
      <div class="column is-3">
        {{ tarefa.projeto?.nome || 'N/D' }}
      </div>
      <div class="column">
        <CronometroCounter :tempoEmSegundos="tarefa.duracaoEmSegundos" />
      </div>
    </div>
  </BoxItem>
</template>

<script lang="ts">
import ITarefa from '@/interfaces/ITarefa'
import { defineComponent, PropType } from 'vue'
import CronometroCounter from './CronometroCounter.vue'
import BoxItem from './BoxItem.vue'

export default defineComponent({
  name: 'TarefaTask',
  components: {
    CronometroCounter,
    BoxItem,
  },
  props: {
    tarefa: {
      type: Object as PropType<ITarefa>,
      required: true,
    },
  },
})
</script>
