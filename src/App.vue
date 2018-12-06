<template>
  <div id="app">
    <Title/>
    <Gameboard
      @newgame="newGame"
      :score="score"
      :winPow="winPow"
      :pieceVal="pieceVal"
      :selectedKeys="selectedKeys"
      :grid="grid"
      :gridsize="gridsize"
    />
    <br>
    <div>Instructions: Use the key shown in the white circle to move the blocks that direction. Blocks combine with like blocks. You're trying to get to 2048.</div>
    <br>
    <div>Based on <a href="https://play2048.co/">2048</a></div>
    <modal name="win"><div class='winnerTitle'><div>A Winner Is You!</div></div></modal>
    <modal name="lose"><div class='winnerTitle'><div>Do you want to play the game again?</div></div></modal>

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
      winPow: 11,
      pieceVal: 2,
      gameOver: false,
      validKeys: "|`~-_=+{[]};:',<>/\\".split(""),
      selectedKeys: []
    };
  },
  methods: {
    show: function() {
    this.$modal.show("lose");
  },
  hide: function() {
    this.$modal.hide("win");
  },
    selectKeys: function(keyToReplace = "none") {
      if (this.selectedKeys.length === 0) {
        for (let index = 0; index < 4; index++) {
          let item = this.validKeys.splice(
            Math.floor(Math.random() * this.validKeys.length),
            1
          );
          this.selectedKeys.push(item[0]);
        }
        return;
      }
      //You need to replace the whole array or vue doesn't count it as updated so just splice won't work
      let ArrCopy = this.selectedKeys.slice();
      ArrCopy.splice(
        ArrCopy.indexOf(keyToReplace),
        1,
        this.validKeys
          .splice(
            Math.floor(Math.random() * this.validKeys.length),
            1,
            keyToReplace
          )
          .toString()
      );
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
      (this.validKeys = "|`~-_=+{[]};:',<>/\\".split("")),
        (this.selectedKeys = []);
      this.selectKeys();
      this.generateRandomSpot();
      this.generateRandomSpot();
    },
    generateRandomSpot: function() {
      if (this.containsZeros()) {
        const emptySpots = this.grid
          .map((e, i) => (e === 0 ? i : ""))
          .filter(Number);
        const randomItem =
          emptySpots[Math.floor(Math.random() * emptySpots.length)];
        this.grid[randomItem] = Math.random() < 0.9 ? 1 : 2;
      }
    },
    movement: function(key) {
      let newArray = [];
      if (this.direction === "left" || this.direction === "right") {
        for (let i = 0; i < this.gridsize; i++) {
          let row = this.grid.slice(
            i * this.gridsize,
            i * this.gridsize + this.gridsize
          );
          if (this.direction === "right") {
            row = row.reverse();
          }
          row = this.rowReduceLeft(row);
          if (this.direction === "right") {
            row = row.reverse();
          }
          newArray = newArray.concat(row);
        }
      }
      if (this.direction === "up" || this.direction === "down") {
        newArray = this.createZeroBoard(this.gridsize);
        for (let i = 0; i < this.gridsize; i++) {
          let row = this.grid.filter((e, loc) => {
            if ((loc + this.gridsize) % this.gridsize === i) {
              return true;
            }
          });
          if (this.direction === "down") {
            row = row.reverse();
          }
          row = this.rowReduceLeft(row);
          if (this.direction === "down") {
            row = row.reverse();
          }
          for (let index = 0; index < this.gridsize; index++) {
            newArray[i + index * this.gridsize] = row[index];
          }
        }
      }
      if (this.grid.toString() !== newArray.toString()) {
        this.grid = newArray;
        this.generateRandomSpot();
        this.selectKeys(key);
      }

      this.gameEndCheck();
    },
    gameEndCheck: function() {
      if (!this.containsZeros() && !this.containsMatchs()) {
        this.gameOver = true;
        this.$modal.show("lose");

        console.log("Lose");
        return;
      }
      if (this.grid.includes(this.winPow)) {
        this.gameOver = true;
        this.$modal.show("win");
        console.log("Win");
        return;
      }
    },
    containsZeros: function() {
      return this.grid.includes(0);
    },
    containsMatchs: function() {
      let returnVal = false;
      this.grid.forEach((e, i) => {
        if (e !== 0) {
          if (
            this.grid[i + 1] === e ||
            this.grid[i - 1] === e ||
            this.grid[i + this.gridsize] === e ||
            this.grid[i - this.gridsize] === e
          ) {
            returnVal = true;
          }
        }
      });
      return returnVal;
    },
    //This was fun to write. It will only work one way so make sure you hand it the array in the right direction. It moves every number to the left, then starts adding them. This emulates the behavior of the actual 2048 game
    rowReduceLeft: function(rowArr) {
      let movesLeft = true;
      do {
        movesLeft = false;
        rowArr.forEach((e, i) => {
          if (i !== 0) {
            if (rowArr[i - 1] === 0 && e !== 0) {
              rowArr[i - 1] += e;
              rowArr[i] = 0;
              movesLeft = true;
            }
          }
        });
      } while (movesLeft);
      rowArr.forEach((e, i) => {
        if (i !== 0) {
          if (rowArr[i - 1] === e && e !== 0) {
            rowArr[i - 1] += 1;
            rowArr.splice(i, 1);
            rowArr.push(0);
            this.score += Math.pow(2, e) * (0.5 * this.pieceVal);
          }
        }
      });
      return rowArr;
    }
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
      else {
        return;
      }
      this.movement(e.key);
    });
  },
  
};
</script>

<style>
html {
  background-color: lightgray;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.winnerTitle{
  display: flex;
  font-size:64px;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  background-color: black;
  color:white;
      border: 15px grey solid;
  border-radius: 6px;
  box-sizing: border-box;
  text-align: center;
  padding: 10px;
}
.v--modal {
  background-color: rgba(0, 0, 0, 0) !important;
}
</style>
