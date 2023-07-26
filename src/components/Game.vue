<template>
  <div>
    <h1>Tic Tac Toe</h1>
    <GameBoard :board="board" @make-move="makeMove" />
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
          <button
            @click="jumpToMove(index)"
            :class="{ 'current-move': index === currentMove }"
          >
            {{ move.position }}({{ move.player }})
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
      moveHistory: [],
      sortAsc: true,
      currentMove: 0 // Điểm bắt đầu của lịch sử nước đi
    };
  },
  computed: {
    sortedMoveHistory() {
      if (this.sortAsc) {
        return this.moveHistory.slice().sort((x, o) => x.move - o.move);
      } else {
        return this.moveHistory.slice().sort((o, x) => o.move - x.move);
      }
    }
  },

    methods: {
      startGame() {
      this.board = Array(this.rows)
        .fill()
        .map(() => Array(this.cols).fill(''));
      this.gameOver = false;
      this.winner = null;
      this.moveHistory = [];
      this.currentMove = -1;
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

      makeMove(row, col) {
    if (!this.gameOver && this.board[row][col] === '') {
      this.board[row][col] = this.currentPlayer;
      this.moveHistory.push({
        move: this.moveHistory.length + 1,
        player: this.currentPlayer,
        position: `(${col}, ${row})`,
        row: row,
        col: col,
        board: this.board.map(row => row.slice()) // Save a copy of the current board state
      });

      if (this.checkWin(row, col)) {
        this.gameOver = true;
        this.winner = this.currentPlayer;
      } else if (this.moveHistory.length === this.rows * this.cols) {
        this.gameOver = true;
      } else {
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
      this.currentMove = this.moveHistory.length - 1;
    }
  },
      jumpToMove(index) {
    // Reset the board to the state of the selected move
    this.board = Array(this.rows)
      .fill()
      .map((_, row) => this.moveHistory[index].board[row].slice());

    // Update currentPlayer based on the selected move
    this.currentPlayer = this.moveHistory[index].player;

    // Update the game status
    this.gameOver = false;
    this.winner = null;
    if (this.checkWin(this.moveHistory[index].row, this.moveHistory[index].col)) {
      this.gameOver = true;
      this.winner = this.currentPlayer;
    } else if (this.moveHistory.length === this.rows * this.cols) {
      this.gameOver = true;
    }

    // Update the currentMove index
    this.currentMove = index;
  },
    toggleSortOrder() {
  this.sortAsc = !this.sortAsc;
},
  },
    
    mounted() {
      this.startGame();
    }
    
  };

  </script>
  <style>
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
  }
  .over{
    background-color:  yellow;
  }
  button{
    background-color: greenyellow;
    
    border-radius: 10px ;
  }
  button:hover{
    background-color: bisque;
  }
</style>
  