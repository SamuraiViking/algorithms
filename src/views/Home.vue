<template>
  <div class="home">
    <div>
      <range-slider
        class="slider"
        min="1"
        :max="containerSize"
        step="1"
        v-model="numOfColumns"
        @input="changeWidthOfColumns()"
      ></range-slider>
    </div>
    <div>Num of Columns: {{ numOfColumns }}</div>
    <div>
      <range-slider class="slider" min="1" max="100" step="1" v-model="timeInterval"></range-slider>
    </div>
    Speed: {{ timeInterval }}
    <div>
      <button @click="createColumns()">Create New Columns</button>
    </div>
    <div>
      <button @click="sortColumns()">Sort Columns</button>
    </div>
    <div class="row">
      <div class="column-container" v-for="column in columns" :key="column.key">
          <div class="column"
               :style="divDimensions(widthOfColumns, column.height)">
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import RangeSlider from "vue-range-slider";
// you probably need to import built-in style
import "vue-range-slider/dist/vue-range-slider.css";

export default {
  name: "home",
  components: {
    RangeSlider
  },
  created() {
    this.createColumns();
  },
  data() {
    return {
      numOfColumns: 800,
      containerSize: 800,
      widthOfColumns: 1,
      timeInterval: 0.0,
      selectedCol: '',
      columns: []
    };
  },
  methods: {
    divDimensions(width, height) {
      return `width: ${width}px; height: ${height}px;`;
    },
    changeHeight() {
      console.log("hello")
    },
    randomNumberBetween(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    changeWidthOfColumns() {
      this.widthOfColumns = this.containerSize / this.numOfColumns;
      this.createColumns();
    },
    createColumns() {
      this.columns = [];
      for (var i = 0; i < this.numOfColumns; i++) {
        var elem = { key: i, height: this.randomNumberBetween(10, 490) }
        this.columns.push(elem);
      }
    },
    sortColumns() {
      this.columns.forEach(function(column) {
        column.height = 1000;
      })
    },
    getRandomColor() {  
      var letters = '0123456789ABCDEF';
      var color = '#';
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    mySort(a, b) {
      return a - b;
    }
  }
};
</script>

<style>
.row {
  width: 800px;
  margin: 0px auto;
  display: flex;
}

.selected {
  background: blue !important;
}

.column-container {
  /* position: relative; */
}

.column {
  bottom: 0px;
  background: black;
}
</style>
