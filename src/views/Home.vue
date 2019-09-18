<template>
  <div class="home">
    <range-slider
      class="slider"
      min="1"
      max="400"
      step="1"
      v-model="numOfColumns"
      @input="changeWidthOfColumns()"
      >
    </range-slider>
    <div>
      Num of Columns: {{ numOfColumns }}
    </div>
    <button @click="createColumns()">Create New Columns</button>
    <button @click="sortColumns()">Sort Columns</button>
    <div class="row">
      <div class="row-container">
        <div v-for="column in columns">
          <div class="column" :style="divDimensions(widthOfColumns, column)"></div>
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
      numOfColumns: 400,
      widthOfColumns: 2,
      columns: [],
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
      this.widthOfColumns = 800 / this.numOfColumns;
      this.createColumns();
    },
    createColumns() {
      this.columns = []
      for(var i = 0; i < this.numOfColumns; i++) {
        this.columns.push(this.randomNumberBetween(1, 500))
      }
    },
    sortColumns() {
      var columns = document.getElementsByClassName("row-container")[0].childNodes
      columns.forEach((column ,index) => {
        if(index) {
          var prevSelectedCol = columns[index - 1].childNodes[0]
        }
        var selectedCol = columns[index].childNodes[0]
        setTimeout(() => {
          if(prevSelectedCol) {
            prevSelectedCol.classList.remove('selected')
          }
          selectedCol.classList.add('selected')
          if(index === this.numOfColumns - 1) {
            setTimeout(() => {
              selectedCol.classList.remove('selected')
            }, 100)
          }
        }, index * 100, selectedCol, prevSelectedCol, index)
      });
    },
    mySort(a, b) {
      return a - b
    }
  }
}
</script>

<style>

.row {
  margin: 100px auto 0px auto;
  background: white;
  width: 50%;
}

.row-container {
  display: flex;
}

.selected {
  background: blue !important;
}

.column {
  background: black;
  border: 1px solid white;
}

</style>
