<template>
    <div class="cell" @click="handleClick">
      {{ value }}
    </div>
  </template>
  
  <script>
  export default {
    name: 'GameCell',
    props: ['value'],
    methods: {
      handleClick() {
        if (!this.value) {
          this.$emit('click');
        }
      }
    }
  };
  </script>
  
  <style>
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
  