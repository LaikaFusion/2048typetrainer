<template>
<div class="gameArea">
  <div class="topRow">
    <div class="keyStyle">{{selectedKeys[0]}}</div>
  </div>
  <div class= "middleRow">
    <div class="keyStyle">{{selectedKeys[1]}}</div>    
    <div :style="style" class="gameboard">
      <div v-for="(cell,index) in grid" :key=index>
        {{cell}}
      </div>
    </div>
    <div class="keyStyle">{{selectedKeys[2]}}</div>    
  </div>
   <div class="bottomRow">
    <div class="keyStyle">{{selectedKeys[3]}}</div>
  </div>
</div>

</template>

<script>
export default {
  name: "Gameboard",
  props: {
    gridsize: Number,
    grid: Array,
    selectedKeys: Array
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
}
.middleRow{
  display: flex;
  justify-content: center;
  align-items: center;
}
.keyStyle{
  margin: 10px;
}
</style>
