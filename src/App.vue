<template>
  
 <div id="app">
  <!-- <img src="https://mir-s3-cdn-cf.behance.net/projects/404/15dc4b69631277.Y3JvcCw4MDgsNjMyLDAsMA.png"> -->
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
#app {
    position: relative;
    display: flex;
    /* justify-content: center;
    align-items: center; */
    /* height: 90px; */
  }

  #app::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 800%;
   /* background-image: url(https://mir-s3-cdn-cf.behance.net/projects/404/15dc4b69631277.Y3JvcCw4MDgsNjMyLDAsMA.png); */
    background-repeat: no-repeat;
    background-size: cover;
    z-index: -1;
  }

  /* .overlay {
    position: relative;
    z-index: 1;
    padding: 20px;
    color: white;
    text-align: center;
  }
  .overlay h1 {
    color: yellow;
  } */

  h1,label,
  input,
  button {
    color: white;
    background-color: rgba(0, 0, 0, 0.5);
    border: none;
    padding: 10px;
    border-radius: 10px;
    text-align: center;
    justify-content: center;
    align-items: center;
  }
</style>
