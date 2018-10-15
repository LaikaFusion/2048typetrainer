<template>
  <div id="app">
    <Title/>
    Score:{{score}}
    <Gameboard :winVal=winVal :selectedKeys=selectedKeys :grid=grid :gridsize=gridsize />
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
      direction: "",
      score: 0,
      winVal: 2048,
      pieceVal: 2,
      gameOver: false,
      validKeys: "|`~-_=+{[]};:',<>/\\".split(''),
      selectedKeys: []
    };
  },
  methods: {
    selectKeys: function(keyToReplace="none"){
      if(this.selectedKeys.length === 0){
        for (let index = 0; index < 4; index++) {
          let item = this.validKeys.splice(Math.floor(Math.random()*this.validKeys.length),1)
          this.selectedKeys.push(item[0]);
        }
        return
      }
      //You need to replace the whole array or vue doesn't count it as updated so just splice won't work
      let ArrCopy = this.selectedKeys.slice();
      ArrCopy.splice(ArrCopy.indexOf(keyToReplace),1, this.validKeys.splice(Math.floor(Math.random()*this.validKeys.length),1,keyToReplace).toString());
      this.selectedKeys = ArrCopy;
    },
    createZeroBoard: function(gridsize) {
      return Array(gridsize * gridsize).fill(0);
    },
    newGame: function() {
      this.grid = this.createZeroBoard(this.gridsize);
      this.direction = "";
      this.score = 0;
      this.gameOver = false;
      this.selectKeys();
      this.generateRandomSpot();
      this.generateRandomSpot();
    },
    generateRandomSpot: function() {
      if(this.containsZeros()){
         const emptySpots = this.grid
        .map((e, i) => (e === 0 ? i : ""))
        .filter(Number);
      const randomItem =
        emptySpots[Math.floor(Math.random() * emptySpots.length)];
      this.grid[randomItem] = Math.random() < 0.9 ? this.pieceVal : (this.pieceVal * 2);
      }
     
    },
    movement: function() {
      let newArray = []
      if(this.direction==="left"||this.direction==="right"){for (let i = 0; i < (this.gridsize); i++) {

        let row = this.grid.slice((i*this.gridsize),(i*this.gridsize)+this.gridsize);
        if(this.direction==="right"){
          row = row.reverse()
        }
        row = this.rowReduceLeft(row);
        if(this.direction==="right"){
          row = row.reverse()
        }
        newArray = newArray.concat(row);
      }}
      if(this.direction==="up"||this.direction==="down"){
        newArray = this.createZeroBoard(this.gridsize);
        for (let i = 0; i < (this.gridsize); i++) {
          let row = this.grid.filter((e,loc)=>{
            if ((loc+this.gridsize) % this.gridsize===i){
              return true
            }
          })         
          if(this.direction==="down"){
            row = row.reverse()
          }
          row = this.rowReduceLeft(row);
          if(this.direction==="down"){
            row = row.reverse()
          }
          for (let index = 0; index < this.gridsize; index++) {
            newArray[i+(index*this.gridsize)] = row[index]
          }
        }
      }
      if(this.grid.toString() !== newArray.toString()){
        this.grid = newArray;
        this.generateRandomSpot();
      }

      this.gameEndCheck()
    },
    gameEndCheck: function(){
      if(!this.containsZeros() && !this.containsMatchs()){
        this.gameOver = true;
        console.log("Lose");
      }
      if(this.grid.includes(2048)){
        this.gameOver = true;
        console.log("Win")
      }
    },
    containsZeros: function(){
      return this.grid.includes(0)
    },
    containsMatchs: function(){
      let returnVal = false
      this.grid.forEach((e,i)=>{
        if(e!==0){
          if (this.grid[i+1]===e||this.grid[i-1]===e||this.grid[i+(this.gridsize)]===e||this.grid[i-(this.gridsize)]===e){
           returnVal =  true
          }
        }
      })
      return returnVal;
    },
    //This was fun to write. It will only work one way so make sure you hand it the array in the right direction. It moves every number to the left, then starts adding them. This emulates the behavior of the actual 2048 game
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
          this.score +=e
        }
      }
    })
    return rowArr; 
  },
  
  },
  beforeMount() {
    this.newGame();
  },
  mounted() {
    window.addEventListener("keydown", e => {
      if(e.key===this.selectedKeys[0]){
        this.direction = "up";
      }
      else if (e.key===this.selectedKeys[1]){
        this.direction = "down";
      }
      else if (e.key ===this.selectedKeys[2]){
        this.direction = "right";      
      }
      else if (e.key === this.selectedKeys[3]){
        this.direction = "left";
      }
      else{
        return;
      }
      console.log(this.selectedKeys);
      this.movement()
      this.selectKeys(e.key)
    });
  },
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
