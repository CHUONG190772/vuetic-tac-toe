<template>
    <div>
        <h1>Tic Tac Toe</h1>
    <Cell :board="board" @make-move="makeMove" />
        <div class="row" v-for="(row, rowIndex) in rows" :key="rowIndex">
        <div
        class="cell"
        v-for="(cell, colIndex) in cols"
        :key="colIndex"
        @click="makeMove(rowIndex, colIndex)"
        >
        {{ board[rowIndex * cols + colIndex] }}
        </div>
      </div>
    </div>
  </template>
  
  
  <script>
  import Cell from './Cell.vue';
  export default {
    name: 'GameBoard',
    components: Cell,
    props: ['board'],
    computed: {
      rows() {
        return Math.sqrt(this.board.length);
      },
      cols() {
        return Math.sqrt(this.board.length);
      }
    },
    methods: {
      makeMove(row, col) {
        this.$emit('make-move', row, col);
      }
    },
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
    background-color: #fff;
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
  </style>
  