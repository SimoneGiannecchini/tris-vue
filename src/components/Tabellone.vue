<script setup>
import { ref, computed, watch } from 'vue'
import Cella from './Cella.vue'
import '../assets/tabellone.css'


const punteggioX = ref(Number(localStorage.getItem('punteggioX')) || 0)
const punteggioO = ref(Number(localStorage.getItem('punteggioO')) || 0)

const celle = ref(Array(9).fill(null))
const turnoX = ref(true)

const vincitore = computed(() => {
  const combinazioni = [
    [0,1,2], [3,4,5], [6,7,8],
    [0,3,6], [1,4,7], [2,5,8],
    [0,4,8], [2,4,6]
  ]
  for (const [a, b, c] of combinazioni) {
    if (celle.value[a] && celle.value[a] === celle.value[b] && celle.value[a] === celle.value[c]) {
      return celle.value[a]
    }
  }
  return null
})


const aggiornato = ref(false)
watch(vincitore, (val) => {
  if (val && !aggiornato.value) {
    if (val === 'X') {
      punteggioX.value++
      localStorage.setItem('punteggioX', punteggioX.value)
    } else if (val === 'O') {
      punteggioO.value++
      localStorage.setItem('punteggioO', punteggioO.value)
    }
    aggiornato.value = true
  }
})

function gestisciClick(index) {
  if (celle.value[index] || vincitore.value) return
  celle.value[index] = turnoX.value ? 'X' : 'O'
  turnoX.value = !turnoX.value
}

function nuovaPartita() {
  celle.value = Array(9).fill(null)
  turnoX.value = true
  aggiornato.value = false
}

function azzeraTutto() {
  punteggioX.value = 0
  punteggioO.value = 0
  localStorage.setItem('punteggioX', 0)
  localStorage.setItem('punteggioO', 0)
  nuovaPartita()
}
</script>

<template>
  <div class="tabellone">
    <h2>Gioco del Tris</h2>

    <div class="scores">
      <p> X: {{ punteggioX }}</p>
      <p> O: {{ punteggioO }}</p>
    </div>

    <div class="griglia">
      <Cella
        v-for="(val, i) in celle"
        :key="i"
        :value="val"
        :index="i"
        @click="gestisciClick"
      />
    </div>

    <div class="status">
      <p v-if="vincitore" class="vincitore-msg"> Vince: {{ vincitore }}!</p>
      <p v-else>Turno: {{ turnoX ? 'X' : 'O' }}</p>
    </div>

    <div class="bottoni">
      <button @click="nuovaPartita"> Nuova Partita</button>
      <button @click="azzeraTutto"> Azzera Tutto</button>
    </div>
  </div>
</template>
