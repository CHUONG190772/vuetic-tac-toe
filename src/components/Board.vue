<template>
    <div>
      <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
        <div
          v-for="(cell, colIndex) in row"
          :key="colIndex"
          :class="{ cell: true, 'cell-x': cell === 'X', 'cell-o': cell === 'O' }"
          @click="makeMove(rowIndex, colIndex)"
        >
          {{ cell }}
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'GameBoard',
    props: ['board'],
    methods: {
      makeMove(row, col) {
        this.$emit('make-move', row, col);
      }
    }
  };
  </script>
  
  <style>
  .row {
    display: flex;
    justify-content: center;
  }
  .winner{
    background-color: black;
  }
  
  .cell {
    /* width: 40px;
    height: 40px; */
    border: 1px solid black;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    font-size: 20px;
    background-color: #afeef3;
    transition: background-color 0.3s ease;
  }
  .cell:hover{
    background-color: black;
  }
  
  .cell-x {
    color: blue;
  }
  
  .cell-o {
    color: red;
  }
  .cell-x:hover{
    color: aqua;
  }
  .cell-over{
    background-color: black;
  }
  button.count-over-2 {
    background-color: black;
    color: white;
  }

  button.count-over-5 {
    background-color: black;
    color: yellow;
  }
div.cell.over{
    background-color: black;
  }
  </style>
  