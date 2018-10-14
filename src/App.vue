<template>
  <div id="app">
    <Title/>
    <Gameboard :grid=grid :gridsize=gridsize />
  </div>
</template>

<script>
import Title from "./components/Title.vue";
import Gameboard from "./components/Gameboard.vue";

export default {
  name: "app",
  components: {
    Title,
    Gameboard
  },
  data: function() {
    return {
      gridsize: 4,
      grid: []
    };
  },
  methods: {
    createZeroBoard: function(gridsize) {
      return Array(gridsize * gridsize).fill(0);
    },
    newGame: function() {
      this.grid = this.createZeroBoard(this.gridsize);
      this.generateRandomSpot();
      this.generateRandomSpot();

    },
    generateRandomSpot: function() {
      const emptySpots = this.grid
        .map((e, i) => (e === 0 ? i : ""))
        .filter(Number);
      const randomItem = emptySpots[Math.floor(Math.random() * emptySpots.length)];
      this.grid[randomItem] = 2
    }
  },
  beforeMount() {
    this.newGame();
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
