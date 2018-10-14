<template>
<div :style="style" class="gameboard">
  <div v-for="(cell,index) in grid" :key=index>
    {{cell}}
  </div>
</div>
</template>

<script>
export default {
  name: "Gameboard",
  props: {
    gridsize: Number,
    grid: Array
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
  margin: auto;
  width: 600px;
  grid-gap: 2%;
  padding: 2%;
}
</style>
