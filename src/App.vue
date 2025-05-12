<template>
  <div class="container">
    <h1>Ultimate Tris</h1>

    <div class="scores">
      <p><strong>Player 1 (X):</strong> {{ scores.X }}</p>
      <p><strong>Player 2 (O):</strong> {{ scores.O }}</p>
    </div>

    <p v-if="winner" class="winner-msg">
       {{ winner === 'draw' ? 'Draw!' : 'The player wins ' + (winner === 'X' ? '1 (X)' : '2 (O)') }} 🏆
    </p>
    <p v-else>Turno: <strong>{{ currentPlayer === 'X' ? 'Player 1 (X)' : 'Player 2 (O)' }}</strong></p>

    <div class="board">
      <div
        v-for="(cell, index) in board"
        :key="index"
        class="cell"
        @click="handleClick(index)"
      >
        {{ cell }}
      </div>
    </div>

    <div class="buttons">
      <button @click="resetBoard"> Reset grid</button>
      <button @click="resetAll"> Reset everything</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(''),
      currentPlayer: 'X',
      winner: null,
      scores: {
        X: 0,
        O: 0,
      },
    };
  },
  methods: {
    handleClick(index) {
      if (this.board[index] === '' && !this.winner) {
        this.board[index] = this.currentPlayer;
        if (this.checkWinner()) {
          this.winner = this.currentPlayer;
          this.scores[this.currentPlayer]++;
        } else if (!this.board.includes('')) {
          this.winner = 'draw';
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
      }
    },
    checkWinner() {
      const winPatterns = [
        [0,1,2], [3,4,5], [6,7,8],
        [0,3,6], [1,4,7], [2,5,8],
        [0,4,8], [2,4,6],
      ];
      return winPatterns.some(([a, b, c]) => {
        return (
          this.board[a] &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        );
      });
    },
    resetBoard() {
      this.board = Array(9).fill('');
      this.winner = null;
      this.currentPlayer = 'X';
    },
    resetAll() {
      this.resetBoard();
      this.scores.X = 0;
      this.scores.O = 0;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  background: #f5f5f5;
}
.container {
  text-align: center;
  padding: 2rem;
}
.scores {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-bottom: 1rem;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 10px;
  justify-content: center;
  margin: 1rem auto;
}
.cell {
  width: 100px;
  height: 100px;
  background: white;
  font-size: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid #ccc;
  cursor: pointer;
}
.winner-msg {
  font-size: 1.3rem;
  color: green;
}
.buttons button {
  margin: 0.5rem;
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
</style>
