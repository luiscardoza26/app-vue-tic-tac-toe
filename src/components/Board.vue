<template>
    <div class="container">
        <h3 v-if="ganador">Ganador: {{ ganador }} </h3>
        <h3 v-else>Jugadores en movimiento: {{ player }} </h3>
        <button @click="reset" class="btn btn-danger text-white mb-3">
            Reiniciar
        </button>

        <div v-for="(_, x) in 3" :key="x" class="row">
            <button v-for="(_, y) in 3" :key="y" class="square" @click="move(x, y)">
                {{ squares[x][y] }}
            </button>
        </div>
        <h3 class="mt-5">Historial</h3>
        <div v-for="(game, idx) in history" :key="idx">
            Juego {{ idx + 1}}: {{ game }} Ganó
        </div>
    </div>
</template>

<script>
import { computed, onMounted, ref, watch } from 'vue'

const calcularGanador = squares => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a]
    }
  }
  return null
}

export default {
  setup () {
    const player = ref('X')
    const squares = ref([
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ])
    const ganador = computed(() => calcularGanador(squares.value.flat()))

    const move = (x, y) => {
      if (ganador.value) return
      squares.value[x][y] = player.value
      player.value = player.value === 'O' ? 'X' : 'O'
    }

    const reset = () => {
      player.value = 'X'
      squares.value = [
        ['', '', ''],
        ['', '', ''],
        ['', '', '']
      ]
    }

    const history = ref([])
    watch(ganador, (current, previous) => {
      if (current && !previous) {
        history.value.push(current)
        localStorage.setItem('history', JSON.stringify(history.value))
      }
    })

    onMounted(() => {
      history.value = JSON.parse(localStorage.getItem('history')) ?? []
    })

    return { ganador, player, squares, move, reset, history }
  }
}
</script>

<style scoped>
.square {
    background: rgb(214, 214, 214);
    border: 2px solid rgb(70, 70, 70);
    box-shadow: rgb(75, 75, 75);
    font-size: 70px;
    font-weight: bold;
    line-height: 34px;
    height: 100px;
    margin-right: -515px;
    margin-left: 515px;
    margin-top: -1px;
    padding: 0;
    text-align: center;
    width: 100px;
}
</style>
