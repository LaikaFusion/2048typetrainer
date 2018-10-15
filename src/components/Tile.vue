<template>
<div :style="style" v-if="number > 0" class="tile">
  {{Math.pow(2,number)*(.5*pieceVal)}}
</div>
<div  v-else class="tile">
  0
</div>

</template>

<script>
export default {
  name: "Tile",
  props: {
    number: Number,
    winPow: Number,
    pieceVal:Number
  },
   computed: {
    style() {
      return {
        backgroundColor:`hsl(${(this.number/this. winPow) *350},100%,50%)` 
      };
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .tile{
    background-color: black;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    color:white;
    font-size: 36px;
    text-shadow: 1px 0 0 #000, 0 -1px 0 #000, 0 1px 0 #000, -1px 0 0 #000;
    border-radius: 6px;
  }
</style>
