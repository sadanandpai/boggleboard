<script>
import Box from "./Box.vue";

export default {
  name: "Board",
  components: {
    Box,
  },
  data() {
    return {
      rows: 6,
      cols: 6,
      searchText: "",
      gridLetter: (() => {
        let arr = new Array(30);
        for (let i = 0; i < 30; i++) arr[i] = new Array(60);
        return arr;
      })(),
      colors: (() => {
        let arr = new Array(30);
        for (let i = 0; i < 30; i++) arr[i] = new Array(60);
        return arr;
      })(),
      componentKey: 0,
      isMobile: (function() {
        var check = false;
        (function(a) {
          if (
            /(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows ce|xda|xiino/i.test(
              a
            )
          )
            check = true;
        })(navigator.userAgent || navigator.vendor || window.opera);
        return check;
      })(),
    };
  },
  methods: {
    randomize: function() {
      this.searchText = "";
      this.resetColors();
      do {
        var key = Math.floor(Math.random() * 1000);
      } while (this.componentKey === key);
      this.componentKey = key;
    },
    resetColors: function() {
      this.colors = (() => {
        let arr = new Array(30);
        for (let i = 0; i < 30; i++) arr[i] = new Array(60);
        return arr;
      })();
    },
  },
  computed: {
    searchGridForWord: function() {
      this.resetColors();
      if (this.searchText === "") return false;
      var text = this.searchText.toLowerCase();

      for (let i = 0; i < this.rows; i++) for (let j = 0; j < this.cols; j++) if (dfs.call(this, 0, i, j)) return true;
      return false;

      function dfs(idx, x, y) {
        if (idx == text.length) return true;
        if (x < 0 || x == this.rows || y < 0 || y == this.cols) return false;
        if (this.gridLetter[x][y] != text[idx]) return false;

        let temp = this.gridLetter[x][y];
        this.gridLetter[x][y] = "*";
        let exists =
          dfs.call(this, idx + 1, x, y + 1) ||
          dfs.call(this, idx + 1, x, y - 1) ||
          dfs.call(this, idx + 1, x + 1, y) ||
          dfs.call(this, idx + 1, x - 1, y);
        this.gridLetter[x][y] = temp;
        if (exists) this.colors[x][y] = true;
        else this.colors[x][y] = undefined;
        this.colors = [...this.colors];
        return exists;
      }
    },
  },
  watch: {
    rows: function(newRows, oldRows) {
      if (+newRows * 25 > window.innerHeight - 190) this.rows = oldRows;
      else this.rows = newRows;
    },
    cols: function(newCols, oldCols) {
      if (+newCols * 25 > window.innerWidth - 50) this.cols = oldCols;
      else this.cols = newCols;
    },
  },
};
</script>

<template>
  <div :key="componentKey">
    <div class="inline-block">Rows: <input type="range" min="6" max="30" v-model="rows" /></div>
    <div class="inline-block">Columns: <input type="range" min="6" max="60" v-model="cols" /></div>
    <br /><br />
    <strong>Search text:</strong> <input type="text" v-model="searchText" v-bind:class="{ success: searchGridForWord }" />
    <span v-if="isMobile"><button class="button button-search">Search</button></span>
    <br /><br />
    <div class="height-30px" v-for="i in +rows" :key="i">
      <div class="inline-block" v-for="j in +cols" :key="i + '' + j">
        <Box v-bind:grid="gridLetter" :i="i - 1" :j="j - 1" v-bind:colors="colors" />
      </div>
    </div>

    <button v-on:click="randomize" class="button button--random">Randomize</button>
  </div>
</template>

<style>
div {
  margin: 0px;
}
.inline-block {
  display: inline-block;
}
.height-30px {
  height: 25px;
}

.button {
  background: black;
  color: white;
  padding: 10px;
  margin: 5px;
  border-radius: 5px;
  box-shadow: 2px 2px 6px 1px gray;
}

.button-random {
  font-size: 15px;
}

.button-search{
  padding: 5px;
  font-size: 12px;
}

.success {
  background: greenyellow;
}

input[type="range"] {
  vertical-align: middle;
}
</style>
