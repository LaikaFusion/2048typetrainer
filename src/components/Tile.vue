<template>
<div class="tile">
  {{number}}
</div>

</template>

<script>
export default {
  name: "Tile",
  props: {
    number: Number,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .tile{
    background-color:white;
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
