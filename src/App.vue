<template>
  <div class="app">
    <div class="board">
      <div
        class="grid"
        v-for="(grid,index) in grids"
        :key="index"
        @click="setGrid(index)"
      >{{setSymbol(grid)}}</div>
      <div class="player">
        <span>player: {{setSymbol(player)}}</span>
        <br>
        <span>winner: {{setSymbol(winner)}}</span>
        <br>
        <button @click="reset">reset</button>
      </div>
    </div>
  </div>
</template>

<script>
const lines = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
];
export default {
  data() {
    return {
      grids: [0, 0, 0, 0, 0, 0, 0, 0, 0],
      player: 1,
      winner: 0
    };
  },
  methods: {
    setGrid(idx) {
      if (this.grids[idx] !== 0 || this.winner !== 0) {
        return;
      }
      this.$set(this.grids, idx, this.player);
      this.player = -this.player;
      //this.grids[idx] = 1; this.$forceUpdate();
      this.winner = this.getWinner();
      console.log(this.winner);
    },
    setSymbol(num) {
      return num === 0 ? "" : num === 1 ? "O" : "X";
    },
    getWinner() {
      let result = lines.reduce((accumulator, [a, b, c]) => {
        console.log(accumulator, [a, b, c]);

        if (accumulator !== 0) return accumulator;
        const sum = this.grids[a] + this.grids[b] + this.grids[c];
        if (sum === 3) return 1;
        if (sum === -3) return -1;
        return 0;
      }, 0);
      return result;
      // for (let i = 0; i < lines.length; i++) {
      //   const line = lines[i];
      //   const [a, b, c] = line;
      //   const sum = this.grids[a] + this.grids[b] + this.grids[c];
      //   if (sum === 3) return 1;
      //   if (sum === -3) return -1;
      // }
      // return 0;
    },
    reset() {
      this.grids = [0, 0, 0, 0, 0, 0, 0, 0, 0];
      this.player = 1;
      this.winner = 0;
    }
  }
};
</script>

<style>
.board {
  display: flex;
  flex-flow: row wrap;
  width: 400px;
  height: 400px;
  align-content: flex-start;
  margin: 20px;
}
.grid {
  width: 33%;
  height: 33%;
  border: 1px solid gray;
  box-sizing: border-box;
  font-size: 40px;
  text-align: center;
  line-height: 120px;
}
.player{
  margin-top: 10px;
  font-size: 20px;
}
</style>
