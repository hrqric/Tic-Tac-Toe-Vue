<template>
  <div class="container">
    <h1>Jogo da Velha</h1>

    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        @click="makeMove(index)"
      >
        {{ cell }}
      </div>
    </div>

    <p class="status" v-if="winnerMessage">{{ winnerMessage }}</p>
    <p class="status" v-else>Vez de: {{ currentPlayer }}</p>

    <button @click="resetGame" class="reset-button">🔁 Reiniciar Jogo</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const board = ref(Array(9).fill(''))
const currentPlayer = ref('X')
const winner = ref(null)

const winningCombos = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6],
]

function makeMove(index) {
  if (board.value[index] === '' && !winner.value) {
    board.value[index] = currentPlayer.value
    checkWinner()
    if (!winner.value) {
      currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
    }
  }
}

function checkWinner() {
  for (const [a, b, c] of winningCombos) {
    if (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      winner.value = board.value[a]
      return
    }
  }

  if (board.value.every(cell => cell !== '')) {
    winner.value = 'Empate'
  }
}

function resetGame() {
  board.value = Array(9).fill('')
  currentPlayer.value = 'X'
  winner.value = null
}

const winnerMessage = computed(() => {
  if (winner.value === 'Empate') return 'Deu velha!'
  if (winner.value) return `Jogador ${winner.value} venceu!`
  return ''
})
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #121212;
  color: #fff;
  font-family: 'Segoe UI', sans-serif;
  text-align: center;
  padding: 2rem;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 10px;
  margin: 2rem 0;
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #fff;
  color: #333;
  font-size: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid #444;
  cursor: pointer;
  user-select: none;
  transition: background-color 0.2s;
}

.cell:hover {
  background-color: #eee;
}

.status {
  font-size: 1.2rem;
  margin-bottom: 1rem;
}

.reset-button {
  margin-top: 1rem;
  padding: 0.75rem 1.5rem;
  font-size: 1.1rem;
  border: none;
  background-color: #ff5252;
  color: white;
  cursor: pointer;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  transition: background-color 0.3s, transform 0.2s;
}

.reset-button:hover {
  background-color: #e53935;
  transform: scale(1.05);
}
</style>
