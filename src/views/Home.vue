<template>
  <div class="home">
    <div>
      <range-slider
        class="slider"
        min="1"
        max="250"
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
    <select v-model="sortType">
      <option>Bubble</option>
      <option>Insertion</option>
      <option>Cocktail</option>
    </select>
    <div>
      <range-slider
        class="slider"
        min="1"
        max="100"
        step="1"
        v-model="timeInterval"
      ></range-slider>
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
      timeInterval: 1,
      selectedCol: '',
      sortType: '',
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
      if(this.sortType === 'Bubble') {
        this.bubbleSort(this.columns)
      } else if(this.sortType === 'Insertion') {
        this.insertionSort(this.columns)
      } else if(this.sortType === 'Cocktail') {
        this.cocktailSort(this.columns)
      }
    },
    swap(arr, firstIndex, secondIndex){
      var temp = arr[firstIndex].height;
      arr[firstIndex].height = arr[secondIndex].height;
      arr[secondIndex].height = temp;
    },
    async bubbleSort(arr){
      var len = arr.length,
          i, j, stop;
      for (i=0; i < len; i++) {
        for (j=0, stop=len-i; j < stop; j++){
          this.selectedCol = arr[j]
          await this.sleep(this.timeInterval);
          if(arr[j + 1]) {
            if (arr[j].height > arr[j+1].height){
              var temp = arr[j].height
              arr[j].height = arr[j + 1].height;
              arr[j + 1].height = temp
            }
          }
        }
      }
    },
    async insertionSort(arr) {
      for (let i = 1; i < arr.length; i++) {
        let key = arr[i].height;
        let j = i - 1;
        while (j >= 0 && arr[j].height > key) {
          this.selectedCol = arr[j]
          await this.sleep(this.timeInterval);
          arr[j + 1].height = arr[j].height;
          j = j - 1;
        }
        arr[j + 1].height = key;
      }
    },
    cocktailSort(arr) {
      var i, left = 0, right = arr.length - 1, temp;
      
      while (left < right) {
        for (i = left; i < right; i++) {
          if (arr[i].height > arr[i + 1].height) {
            this.swap(arr, i, i+1);
          }
        }
        right--;
        for (i = right; i > left; i--) {
          if (arr[i - 1].height > arr[i].height) {
              this.swap(arr, i-1, i);
          }
        }
        left++;
      }
    },
    getRandomColor() {  
      var letters = '0123456789ABCDEF';
      var color = '#';
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
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
