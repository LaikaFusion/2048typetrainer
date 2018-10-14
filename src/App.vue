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
      grid: [],
      direction: ""
    };
  },
  methods: {
    createZeroBoard: function(gridsize) {
      return Array(gridsize * gridsize).fill(0);
    },
    newGame: function() {
      this.grid = this.createZeroBoard(this.gridsize);
      this.direction = ""
      this.generateRandomSpot();
      this.generateRandomSpot();
    },
    generateRandomSpot: function() {
      const emptySpots = this.grid
        .map((e, i) => (e === 0 ? i : ""))
        .filter(Number);
      const randomItem =
        emptySpots[Math.floor(Math.random() * emptySpots.length)];
      this.grid[randomItem] = Math.random() < 0.9 ? 2 : 4;
    },
    movement: function(direction) {
      if(this.direction==="left"||this.direction==="right"){for (let i = 0; i < (this.gridsize); i++) {
        let row = this.grid.slice((i*this.gridsize),(i*this.gridsize)+this.gridsize);
        if(this.direction==="right"){
          row = row.reverse()
        }
        row = this.rowReduceLeft(row);
        if(this.direction==="right"){
          row = row.reverse()
        }
        Array.prototype.splice.apply(this.grid, [i*this.gridsize, row.length].concat(row));
      }}
      
            if(this.direction==="up"||this.direction==="down"){
      for (let i = 0; i < (this.gridsize); i++) {
        let row = this.grid.slice((i*this.gridsize),(i*this.gridsize)+this.gridsize);
        if(this.direction==="right"){
          row = row.reverse()
        }
        row = this.rowReduceLeft(row);
        if(this.direction==="right"){
          row = row.reverse()
        }
        Array.prototype.splice.apply(this.grid, [i*this.gridsize, row.length].concat(row));
      }}
    },
    containsZeros: function(){
      return this.grid.includes(0)
    },
    containsMatchs: function(){
      this.grid.forEach((e,i)=>{
        if(e!==0){
          if (this.grid[i+1]===e||this.grid[i-1]===e||this.grid[i+(this.gridsize -1)]||this.grid[i+(this.gridsize -1)]){
           return true
          }
        }
      })
      return false
    },
    rowReduceLeft: function(rowArr){
    let movesLeft= true;
    do{
      movesLeft=false
        rowArr.forEach((e, i) => {
          if(i !==0){
             if (
            (rowArr[i - 1] === 0 ) && e !==0
          ) {
            rowArr[i - 1] += e;
            rowArr[i]=0
            movesLeft=true;
          }
          }
         
        });
    }while(movesLeft)
    rowArr.forEach((e,i)=>{
      if(i!==0){
        if(rowArr[i-1]===e){
          rowArr[i - 1] += e;
          rowArr.splice(i,1)
          rowArr.push(0)
        }
      }
    })
    return rowArr; 
  },
  
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
