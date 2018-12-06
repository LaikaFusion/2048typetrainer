<template>
<div class="gameArea">
  <div class="row">
    <div class="score">Score:{{score}}</div>
   
    <div v-on:click="$emit('newgame')" class="newGameButton"><span>New Game</span></div>
  </div>
    <div class="row">

  <div class="gamecontainer">
   
    <div class="keyStyle">{{selectedKeys[0]}}</div>
    
    <div class='middle'>
    <div class="keyStyle">{{selectedKeys[3]}}</div>    
    <div :style="style" class="gameboard">
      <div v-for="(cell,index) in grid" :key=index>
        <Tile :pieceVal=pieceVal :winPow=winPow v-bind:number=cell />
      </div>
    </div>
        <div class="keyStyle">{{selectedKeys[2]}}</div>  

    </div>
        <div class="keyStyle">{{selectedKeys[1]}}</div>
  
  </div>
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
  height: 60vh;
  width: 60vh;
  grid-gap: 2%;
  padding: 1em;
  border-radius: 6px;
}
.row{
  display: flex;
  justify-content: center;
  align-items: center;
  margin: auto;

}
.keyStyle{
  font-size: 35px;
  height: 1.4em;
  width: 1.4em;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color:white;
  border-radius: 1em;

}
.score{
  width: 150px;
  font-size: 30px;
}
.newGameButton{
  font-size:30px;
  background-color: black;
  width: 150px;
  height: 3em;
  border: 10px grey solid;
  border-radius: 6px;
  color:white;
  box-sizing: border-box;
  cursor: pointer;
  text-align: center;
  margin-left: 1em;
  
}
.hiddenDiv{
  width:150px
}
.gamecontainer{
  background-color:gray;

  display:flex;
  flex-direction: column;
  align-items:center;
  margin-top:2rem;
  padding: 1em;
  border-radius: 6px;
}
.middle{
  display:flex;
  align-items: center;
  justify-content: center;
}
</style>
