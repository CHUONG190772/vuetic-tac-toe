<template>
 <div id="app">
    <div v-if="!gameStarted">
      <h1>Tic Tac Toe</h1>
      <label for="rows">Số hàng: </label>
      <input type="number" id="rows" v-model.number="rows">
      <label for="cols"> Số cột: </label>
      <input type="number" id="cols" v-model.number="cols">
      <button @click="startGame">Bắt đầu</button>
    </div>
    <div v-else>
      <Game :rows="rows" :cols="cols" />
    </div>
  </div>
</template>

<script>
import Game from './components/Game.vue';

export default {
  name: 'App',
  components: {
    Game
  },
  data() {
    return {
      gameStarted: false,
      rows: 3,
      cols: 3
    };
  },
  methods: {
    startGame() {
      this.gameStarted = true;
    }
  }
};
</script>
<style scoped>
button:hover{
  background-color: aquamarine;
}
</style>
