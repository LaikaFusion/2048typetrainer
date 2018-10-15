<template>
<div class="gameArea">
  <div class="row">
    <div class="score">Score:{{score}}</div>
    <div class="keyStyle">{{selectedKeys[0]}}</div>
    <div v-on:click="$emit('newgame')" class="newGameButton">New Game</div>
  </div>
  <div class= "row">
    <div class="keyStyle">{{selectedKeys[3]}}</div>    
    <div :style="style" class="gameboard">
      <div v-for="(cell,index) in grid" :key=index>
        <Tile :pieceVal=pieceVal :winPow=winPow v-bind:number=cell />
      </div>
    </div>
    <div class="keyStyle">{{selectedKeys[2]}}</div>    
  </div>
   <div class="row">
     <div class="hiddenDiv"> </div>
    <div class="keyStyle">{{selectedKeys[1]}}</div>
    <div class="hiddenDiv"> </div>
  </div>
</div>

</template>

<script>

import Tile from "./Tile.vue";


export default {
  name: "Gameboard",
  components: {
    Tile
  },
  props: {
    gridsize: Number,
    grid: Array,
    selectedKeys: Array,
    winPow:Number,
    pieceVal:Number,
    score:Number
  },
  computed: {
    style() {
      return {
        gridTemplateColumns: `repeat(${this.gridsize}, 1fr)`,
        gridTemplateRows: ` repeat(${this.gridsize}, 1fr)`
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.gameboard {
  background-color: grey;
  display: grid;
  height: 600px;
  width: 600px;
  grid-gap: 2%;
  padding: 2%;
  border-radius: 6px;
}
.row{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 70%;
  margin: auto;

}
.keyStyle{
  margin: 10px;
  font-size: 40px;
  background-color: white;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 20px grey solid;
  border-radius: 6px;
}
.score{
  width: 150px;
  font-size: 30px;
}
.newGameButton{
  font-size:30px;
  background-color: black;
  width: 150px;
  border: 10px grey solid;
  border-radius: 6px;
  color:white;
  box-sizing: border-box;
  cursor: pointer;
}
.hiddenDiv{
  width:150px
}
</style>
