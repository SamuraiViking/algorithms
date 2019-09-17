<template>
  <div class="home">
    <range-slider
      class="slider"
      min="1"
      max="800"
      step="1"
      v-model="numOfColumns"
      @input="changeWidthOfColumns()"
      >
    </range-slider>
    <div>
      Num of Columns: {{ numOfColumns }}
    </div>
    <button @click="createColumns()">Create New Columns</button>
    <div class="row">
      <div class="row-container">
        <div v-for="column in columns">
          <div class="column" :style="divDimensions(widthOfColumns, column)">
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
    this.createColumns()
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
      this.columns = []
      for(var i = 0; i <= 800; i++) {
        this.columns.push(this.randomNumberBetween(1, 500))
      }
    }
  }
}
</script>

<style>

.row {
  margin: 100px auto 0px auto;
  background: black;
  width: 800px;
  overflow: hidden;
}

.row-container {
  display: flex;
}

.column {
  background: white;
  /* background: black; */
}

</style>
