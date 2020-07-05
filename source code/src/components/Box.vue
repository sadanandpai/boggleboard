<template>
  <div class="box" v-bind:class="{ active: colors[i][j] }">{{ letter }}</div>
</template>

<style>
.box {
  height: 25px;
  width: 25px;
  border: 1px solid black;
  padding: 2px;
}

.active{
    background: yellow;
}
</style>

<script>
export default {
  name: "Box",
  props: {
    grid: Array,
    colors: Array,
    i: Number,
    j: Number
  },
  data() {
    let letter = String.fromCharCode(97 + Math.floor(Math.random() * 26));
    this.grid[this.i][this.j] = letter;
    return {
      letter: letter,
    };
  },
  created: function() {},
};
</script>
