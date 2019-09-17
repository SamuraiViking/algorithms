<template>
  <div class="home">
    <range-slider
      class="slider"
      min="10"
      max="800"
      step="10"
      v-model="numOfColumns"
      @change="changeWidthOfColumns()">
    </range-slider>
    <div>
      {{ numOfColumns }}
    </div>
    <div class="row">
      <div class="row-container">
        <div v-for="column in columns">
          <div class="column" :style="divDimensions(widthOfColumns, column.length)">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import RangeSlider from 'vue-range-slider'
// you probably need to import built-in style
import 'vue-range-slider/dist/vue-range-slider.css'

export default {
  name: 'home',
  components: {
    RangeSlider
  },
  created() {
    for(var i = 0; i < this.numOfColumns; i++) {
      this.columns.push({ length: this.randomNumberBetween(0, 800) })
    }
  },
  data() {
    return {
      numOfColumns: 800,
      widthOfColumns: 1,
      columns: []
    }
  },  
  methods: {
    divDimensions(width, height) {
      return `width: ${width}px; height: ${height}px;`
    },
    randomNumberBetween(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    changeWidthOfColumns() {
      console.log("changing");
      this.widthOfColumns = 800 / this.numOfColumns;
    },
    createColumns() {

    }
  }
}
</script>

<style>

.row {
  /* background: black; */
  width: 800px;
  margin: 0px auto;
  overflow: hidden;
}

.row-container {
  display: flex;
}

.column {
  background: black;
  /* border-left: 1px solid black; */
}

</style>
