<template>
    <div>
      <h1>Tic Tac Toe</h1>
      <GameBoard :board="board" @make-move="makeMove" />
      <div v-if="gameOver">
        <p v-if="winner">Winner: {{ winner }}</p>
        <p v-else>It's a draw!</p>
        <button @click="resetGame">Play Again</button>
      </div>
      <div>
        <h2>Move History</h2>
        <ul>
          <li v-for="(move, index) in moveHistory" :key="index">{{ move }}</li>
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
        moveHistory: []
      };
    },
    methods: {
      startGame() {
        this.board = Array(this.rows).fill().map(() => Array(this.cols).fill(''));
        this.gameOver = false;
        this.winner = null;
        this.moveHistory = [];
        // window.addEventListener('keydown', this.handleKeyDown);
      },
//       handleKeyDown(event) {
//     const key = event.key;
//     const validKeys = ['1', '2', '3', '4', '5', '6', '7', '8', '9'];

//     if (validKeys.includes(key)) {
//       const index = parseInt(key) - 1;
//       const row = Math.floor(index / 3);
//       const col = index % 3;

//       if (!this.gameOver && this.board[row][col] === '') {
//         this.makeMove(row, col);
//       }
//     }
//   },
      makeMove(row, col) {
        if (!this.gameOver && this.board[row][col] === '') {
          this.board[row][col] = this.currentPlayer;
          this.moveHistory.push(`Player ${this.currentPlayer}: (${row}, ${col})`);
          if (this.checkWin(row, col)) {
            this.gameOver = true;
            this.winner = this.currentPlayer;
          } else if (this.isBoardFull()) {
            this.gameOver = true;
          } else {
            this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
          }
        }
      },
//       beforeUnmount() {
//   window.removeEventListener('keydown', this.handleKeyDown);
// },
      checkWin(row, col) {
        // const player = this.board[row][col];
        // let rowCount = 0;
        // let colCount = 0;
        // let diagCount = 0;
        // let antiDiagCount = 0;
  
        // for (let i = 0; i < this.board.length; i++) {
        //   if (this.board[row][i] === player) rowCount++;
        //   if (this.board[i][col] === player) colCount++;
        //   if (this.board[i][i] === player) diagCount++;
        //   if (this.board[i][this.board.length - 1 - i] === player) antiDiagCount++;
        // }
  
        // return (
        //   rowCount === this.board.length ||
        //   colCount === this.board.length ||
        //   diagCount === this.board.length ||
        //   antiDiagCount === this.board.length
        // );
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
      }
    },
    mounted() {
      this.startGame();
    }
  };
  </script>
  