<template>
  <div class="home">
    <div>
      <range-slider
        class="slider"
        min="1"
        :max="250"
        step="1"
        v-model="numOfColumns"
        @input="changeWidthOfColumns()"
      ></range-slider>
    </div>
    <div>Num of Columns: {{ numOfColumns }}</div>
    <div>
      <button @click="createColumns()">Create New Columns</button>
    </div>
    <div>
      <button @click="sortColumns()">Sort Columns</button>
    </div>
    <div class="row">
      <div class="column-container" v-for="column in columns" :key="column.key">
          <div v-if="selectedCol===column"
               class="column selected"
               :style="divDimensions(widthOfColumns, column.changeHeight)"
          >
          </div>
          <div v-else 
               class="column"
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
    this.widthOfColumns = this.containerSize / this.numOfColumns
  },
  data() {
    return {
      numOfColumns: 100,
      containerSize: 800,
      widthOfColumns: '',
      timeInterval: 0.0,
      selectedCol: '',
      columns: []
    };
  },
  methods: {
    sleep (milliseconds) {
      return new Promise(resolve => setTimeout(resolve, milliseconds))
    },
    divDimensions(width, height) {
      return `width: ${width}px; height: ${height}px;`;
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
      this.bubbleSort(this.columns)
    },
    swap(arr, first_Index, second_Index){
        var temp = arr[first_Index].height;
        arr[first_Index].height = arr[second_Index].height;
        arr[second_Index] = temp;
    },
    bubbleSort(arr){
      var len = arr.length,
          i, j, stop;
      for (i=0; i < len; i++) {
        for (j=0, stop=len-i; j < stop; j++){
          setTimeout(this.oneBubbleSort.bind(null, arr, j ,i), 100);
        }
      }
    },
    oneBubbleSort(arr, j, i) {
      console.log(i)
      this.selectedCol = arr[j]
      if(arr[j + 1]) {
        if (arr[j].height > arr[j+1].height){
          var temp = arr[j].height
          arr[j].height = arr[j + 1].height;
          arr[j + 1].height = temp
        }
      }
    },
    insertionSort(arr) {
        let length = arr.length;
        for (let i = 1; i < length; i++) {
            let key = arr[i];
            let j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j = j - 1;
            }
            arr[j + 1] = key;
        }
        return arr;
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
  background: red !important;
}

.column-container {
  /* position: relative; */
}

.column {
  bottom: 0px;
  background: black;
}
</style>
