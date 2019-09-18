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
        >
      </range-slider>
    </div>
    <div>
      Num of Columns: {{ numOfColumns }}
    </div>
    <div>
      <range-slider
        class="slider"
        min="1"
        max="100"
        step="1"
        v-model="timeInterval"
        >
      </range-slider>
    </div>
      Speed: {{ timeInterval }}
    <div>
      <button @click="createColumns()">Create New Columns</button>
    </div>
    <div>
      <button @click="sortColumns()">Sort Columns</button>
    </div>
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
      containerSize: 800,
      widthOfColumns: 2,
      timeInterval: 2,
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
      this.widthOfColumns = this.containerSize / this.numOfColumns;
      this.createColumns();
    },
    createColumns() {
      this.columns = []
      for(var i = 0; i < this.numOfColumns; i++) {
        this.columns.push(this.randomNumberBetween(25, 500))
      }
    },
    sortColumns() {
      var columns = document.getElementsByClassName("row-container")[0].childNodes
      columns.forEach((column ,index) => {
        if(index - 1 >= 0) {
          var prevSelectedCol = columns[index - 1].childNodes[0]
        }
        if(index - 2 >= 0) {
          var prevPrevSelectedCol = columns[index - 2].childNodes[0]
        }
        var selectedCol = columns[index].childNodes[0]
        setTimeout(() => {
          if(prevPrevSelectedCol) {
            prevPrevSelectedCol.classList.remove('selected');
          }
          if(prevSelectedCol) {
            prevSelectedCol.classList.add('selected')
          }

          selectedCol.classList.add('selected')
          if(index === this.numOfColumns - 1) {
            setTimeout(() => {
              selectedCol.classList.remove('selected')
              prevSelectedCol.remove('selected')
            }, this.timeInterval)
          }
        }, index * this.timeInterval, selectedCol, prevSelectedCol, index)
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
  margin: 0px auto;
  width: 800px;
  background: white;
}

.row-container {
  display: flex;
}

.selected {
  background: blue !important;
}

.column {
  background: black;
}

</style>
