<template>
    <div>
      <h1>Tic Tac Toe</h1>
      <div>
        <label for="rows">Rows:</label>
        <input type="number" id="rows" v-model="rows" min="1" max="10">
        <label for="columns">Columns:</label>
        <input type="number" id="columns" v-model="columns" min="1" max="10">
        <button @click="startGame">Start Game</button>
      </div>
      <div v-if="gameStarted">
        <div v-for="(cell, cellIndex) in row" :key="cellIndex" class="cell" @click="makeMove(rowIndex, cellIndex)">
  {{ cell }}
</div>
<!-- <div v-for="(cell, cellIndex) in row" :key="cellIndex" class="cell" @click="makeMove(rowIndex, cellIndex)">
  {{ cell }}
</div> -->
        
      </div>
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
  export default {
    name: 'TicTacToe',
    data() {
      return {
        rows: 3,
        columns: 3,
        currentPlayer: 'X',
        board: [],
        gameOver: false,
        winner: null,
        moveHistory: []
      };
    },
    computed: {
      gameStarted() {
        return this.board.length > 0;
      }
    },
    methods: {
      startGame() {
        this.board = Array(this.rows).fill(Array(this.columns).fill(''));
        this.gameOver = false;
        this.winner = null;
        this.moveHistory = [];
      },
      makeMove(row, col) {
        if (!this.gameOver && this.board[row][col] === '') {
          this.board[row].splice(col, 1, this.currentPlayer);
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
      checkWin(row, col) {
        const player = this.board[row][col];
        let rowCount = 0;
        let colCount = 0;
        let diagCount = 0;
        let antiDiagCount = 0;
        
        for (let i = 0; i < this.board.length; i++) {
          if (this.board[row][i] === player) rowCount++;
          if (this.board[i][col] === player) colCount++;
          if (this.board[i][i] === player) diagCount++;
          if (this.board[i][this.board.length - 1 - i] === player) antiDiagCount++;
        }
        
        return rowCount === this.board.length || colCount === this.board.length ||
          diagCount === this.board.length || antiDiagCount === this.board.length;
      },
      isBoardFull() {
        return this.board.every(row => row.every(cell => cell !== ''));
      },
      resetGame() {
        this.rows = 3;
        this.columns = 3;
        this.currentPlayer = 'X';
        this.board = [];
        this.gameOver = false;
        this.winner = null;
        this.moveHistory = [];
      }
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
  </style>
  