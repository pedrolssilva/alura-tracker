<template>
  <div class="is-flex is-align-items-center is-justify-content-space-between">
    <CronometroCounter :tempoEmSegundos="tempoEmSegundos" />
    <BotaoButton
      @clicado="iniciar"
      icone="fas fa-play"
      texto="play"
      :desabilitado="cronometroRodando"
    />
    <BotaoButton
      @clicado="finalizar"
      icone="fas fa-stop"
      texto="stop"
      :desabilitado="!cronometroRodando"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import CronometroCounter from './CronometroCounter.vue'
import BotaoButton from './BotaoButton.vue'

export default defineComponent({
  name: 'TemporizadorCounter',
  emits: ['aoTemporizadorFinalizado'],
  components: {
    CronometroCounter,
    BotaoButton,
  },
  data() {
    return {
      tempoEmSegundos: 0,
      cronometro: 0,
      cronometroRodando: false,
    }
  },
  methods: {
    iniciar() {
      this.cronometroRodando = true
      this.cronometro = setInterval(() => {
        this.tempoEmSegundos += 1
      }, 1000)
      console.log('iniciando')
    },
    finalizar() {
      this.cronometroRodando = false
      clearInterval(this.cronometro)
      this.$emit('aoTemporizadorFinalizado', this.tempoEmSegundos)
      this.tempoEmSegundos = 0
    },
  },
})
</script>
