<template>
  <div>
    <h1>Tic Tac Toe</h1>
    <GameBoard :board="board" @make-move="makeMove" :winning-squares="winningSquares" />
    <div v-if="gameOver">
      <p v-if="winner" class="win">Winner: {{ winner }}</p>
      <p v-else>It's a draw!</p>
      <button @click="resetGame">Play Again</button>
    </div>
    <div>
      <h2>Move History</h2>
      <button @click="toggleSortOrder">Sort</button>
      <ul>
        <li v-for="(move, index) in sortedMoveHistory" :key="index">
          <button @click="jumpToMove(index)" :class="{ 'current-move': index === currentMove }">
            {{ getDescription(move) }}
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import GameBoard from './Board.vue';

export default {
  name: 'TicTacToeGame',
  components: {
    GameBoard
  },
  props: ['rows', 'cols'],
  data() {
    return {
      board: [],
      currentPlayer: 'X',
      gameOver: false,
      winner: null,
      winningSquares: [],
      moveHistory: [],
      sortAsc: true,
      currentMove: 0
    };
  },
  computed: {
    sortedMoveHistory() {
      if (this.sortAsc) {
        return this.moveHistory.slice().sort((a, b) => a.move - b.move);
      } else {
        return this.moveHistory.slice().sort((a, b) => b.move - a.move);
      }
    },
  },
  methods: {
    startGame() {
      this.board = Array(this.rows)
        .fill()
        .map(() => Array(this.cols).fill(''));
      this.gameOver = false;
      this.winner = null;
      this.winningSquares = [];
      this.moveHistory = [];
      this.currentMove = -1;
    },
    makeMove(row, col) {
      if (!this.gameOver && this.board[row][col] === '') {
        this.board[row][col] = this.currentPlayer;
        this.moveHistory.push({ move: this.moveHistory.length + 1, player: this.currentPlayer, position: `(${col}, ${row})` });
        if (this.checkWin(row, col)) {
          this.gameOver = true;
          this.winner = this.currentPlayer;
          this.highlightWinningSquares(row, col);
        } else if (this.isBoardFull()) {
          this.gameOver = true;
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
        }
        this.currentMove = this.moveHistory.length - 1;
      }
    },
    checkWin(row, col) {
      const player = this.board[row][col];
      // Kiểm tra hàng ngang
      let count = 1;
      let i = col - 1;
      while (i >= 0 && this.board[row][i] === player) {
        count++;
        i--;
      }
      i = col + 1;
      while (i < this.cols && this.board[row][i] === player) {
        count++;
        i++;
      }
      if (count >= 3) {
        return true;
      }
      // Kiểm tra hàng dọc
      count = 1;
      let j = row - 1;
      while (j >= 0 && this.board[j][col] === player) {
        count++;
        j--;
      }
      j = row + 1;
      while (j < this.rows && this.board[j][col] === player) {
        count++;
        j++;
      }
      if (count >= 3) {
        return true;
      }
      // Kiểm tra đường chéo chính (\)
      count = 1;
      i = col - 1;
      j = row - 1;
      while (i >= 0 && j >= 0 && this.board[j][i] === player) {
        count++;
        i--;
        j--;
      }
      i = col + 1;
      j = row + 1;
      while (i < this.cols && j < this.rows && this.board[j][i] === player) {
        count++;
        i++;
        j++;
      }
      if (count >= 3) {
        return true;
      }
      // Kiểm tra đường chéo phụ (/)
      count = 1;
      i = col + 1;
      j = row - 1;
      while (i < this.cols && j >= 0 && this.board[j][i] === player) {
        count++;
        i++;
        j--;
      }
      i = col - 1;
      j = row + 1;
      while (i >= 0 && j < this.rows && this.board[j][i] === player) {
        count++;
        i--;
        j++;
      }
      if (count >= 3) {
        return true;
      }
      return false;
    },
    isBoardFull() {
      return this.board.every((row) => row.every((cell) => cell !== ''));
    },
    resetGame() {
      this.startGame();
    },
    jumpToMove(index) {
      this.currentMove = index;
      if (this.moveHistory[index] && this.moveHistory[index].position) {
        const { position } = this.moveHistory[index];
        const [, col, row] = position.match(/\d+/g);
        if (Array.isArray(this.board) && this.board[row] && this.board[row][col]) {
          this.board = this.moveHistory
            .slice(0, index + 1)
            .reduce((prevBoard, move) => {
              const { position, player } = move;
              const [, moveCol, moveRow] = position.match(/\d+/g);
              prevBoard[parseInt(moveRow)][parseInt(moveCol)] = player;
              return prevBoard;
            }, Array(this.rows).fill().map(() => Array(this.cols).fill('')));
        }
        this.currentPlayer = this.moveHistory[index].player === 'X' ? 'O' : 'X';
        this.gameOver = false;
        this.winner = null;
        this.winningSquares = [];
      }
    },
    toggleSortOrder() {
      this.sortAsc = !this.sortAsc;
    },
    getDescription(move) {
      if (move > 0) {
        return `Go to move #${move} ${this.moveHistory[move - 1].position} (${this.moveHistory[move - 1].player})`;
      } else {
        return 'Go to game start';
      }
    },
    highlightWinningSquares(row, col) {
      const player = this.board[row][col];
      this.winningSquares = [];
      // Kiểm tra hàng ngang
      let count = 1;
      let i = col - 1;
      while (i >= 0 && this.board[row][i] === player) {
        this.winningSquares.push({ row, col: i });
        i--;
      }
      i = col + 1;
      while (i < this.cols && this.board[row][i] === player) {
        this.winningSquares.push({ row, col: i });
        i++;
      }
      if (count >= 3) {
        return;
      }
      // Kiểm tra hàng dọc
      count = 1;
      let j = row - 1;
      while (j >= 0 && this.board[j][col] === player) {
        this.winningSquares.push({ row: j, col });
        j--;
      }
      j = row + 1;
      while (j < this.rows && this.board[j][col] === player) {
        this.winningSquares.push({ row: j, col });
        j++;
      }
      if (count >= 3) {
        return;
      }
      // Kiểm tra đường chéo chính (\)
      count = 1;
      i = col - 1;
      j = row - 1;
      while (i >= 0 && j >= 0 && this.board[j][i] === player) {
        this.winningSquares.push({ row: j, col: i });
        i--;
        j--;
      }
      i = col + 1;
      j = row + 1;
      while (i < this.cols && j < this.rows && this.board[j][i] === player) {
        this.winningSquares.push({ row: j, col: i });
        i++;
        j++;
      }
      if (count >= 3) {
        return;
      }
      // Kiểm tra đường chéo phụ (/)
      count = 1;
      i = col + 1;
      j = row - 1;
      while (i < this.cols && j >= 0 && this.board[j][i] === player) {
        this.winningSquares.push({ row: j, col: i });
        i++;
        j--;
      }
      i = col - 1;
      j = row + 1;
      while (i >= 0 && j < this.rows && this.board[j][i] === player) {
        this.winningSquares.push({ row: j, col: i });
        i--;
        j++;
      }
      if (count >= 3) {
        return;
      }
    },
  },

  mounted() {
    this.startGame();
  }
};
</script>

<style scoped>
.row {
  display: flex;
}

.cell {
  width: 40px;
  height: 40px;
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  font-size: 20px;
  background-color: #afeef3;
  transition: background-color 0.3s ease;
}

.cell:hover {
  background-color: black;
}

.cell-x {
  color: blue;
}

.cell-o {
  color: red;
}

.cell-x:hover {
  color: aqua;
}

.win {
  font-weight: bold;
  font-size: 24px;
  color: green;
}

.current-move {
  font-weight: bold;
}

.game-over {
  font-size: 20px;
  font-weight: bold;
}

.game-over p {
  margin: 0;
}

.game-over button {
  margin-top: 10px;
}
</style>