<template>
  <div class="home">
    <div id="home-container">
      <div id="selectors">
        <div id="selectors-container">
          <div id="slider-selectors">
            <div id="columns-selector">
              <div class="mt-2">Columns: {{ numOfColumns}} </div>
              <b-form-input :disabled="isSorting" id="range-1" v-model="numOfColumns" type="range" min="10" max="200" @change="createColumns"></b-form-input>
            </div>
            <div id="selectors-spacing"></div>
            <div id="speed-selector">
              <div class="mt-2">Speed: {{ timeInterval }}ms </div>
              <b-form-input :disabled="isSorting" id="range-2" v-model="timeInterval" type="range" min="1" max="100"></b-form-input>
            </div>
          </div>
          <div>
            <b-form-select v-model="sortType" :options="sortTypes"></b-form-select>
          </div>
          <div id="action-btns">
            <b-button id="sort-btn" @click="sortColumns()">Sort</b-button>
            <div id="btn-space"></div>
            <b-button id="reset-btn" @click="createColumns()">Reset</b-button>
          </div>
        </div>
      </div>
    </div>
    <div class="myColumns">
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
    window.addEventListener('resize', this.handleResize)
    this.handleResize();
    this.createColumns();
  },
  destroyed() {
    window.removeEventListener('resize', this.handleResize)
  },
  data() {
    return {
      window: {
        width: 0,
        height: 0,
      },
      sortTypes: [
        // { value: null, text: '-- Select a Sorting Algorithm --' },
        { value: 'Bubble', text: 'Bubble' },
        { value: 'Insertion', text: 'Insertion' },
        { value: 'Cocktail', text: 'Cocktail' },
      ],
      isSorting: false,
      numOfColumns: 50,
      containerSize: 800,
      widthOfColumns: 0,
      timeInterval: 1,
      selectedCol: '',
      sortType: 'Insertion',
      columns: []
    };
  },
  methods: {
    handleResize() {
      this.window.width = window.innerWidth
      this.window.height = window.innerHeight
      // this.widthOfColumns = Math.floor(this.window.width / this.numOfColumns)
      this.widthOfColumns = this.window.width / this.numOfColumns
      console.log(this.widthOfColumns)
    },
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
      this.widthOfColumns = Math.ceil(this.window.width / this.numOfColumns);
      this.createColumns();
    },
    createColumns() {
      this.isSorting = false;
      this.columns = [];
      for (var i = 0; i < this.numOfColumns; i++) {
        var elem = { key: i, height: this.randomNumberBetween(10, this.window.height - 330) }
        this.columns.push(elem);
      }
      this.widthOfColumns = Math.ceil(this.window.width / this.numOfColumns);
    },
    sortColumns() {
      this.isSorting = true
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
    async cocktailSort(arr) {
      var i, left = 0, right = arr.length - 1, temp;
      
      while (left < right) {
        for (i = left; i < right; i++) {
          this.selectedCol = arr[i]
          await this.sleep(this.timeInterval)
          if (arr[i].height > arr[i + 1].height) {
            this.swap(arr, i, i+1);
          }
        }
        right--;
        for (i = right; i > left; i--) {
          this.selectedCol = arr[i]
          await this.sleep(this.timeInterval)
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

            /* <div id="columns-selector">
              <div class="mt-2">Columns: {{ numOfColumns}} </div>
              <b-form-input id="range-1" v-model="numOfColumns" type="range" min="10" max="200" @change="createColumns"></b-form-input>
            </div>
            <div id="selectors-spacing"></div>
            <div id="speed-selector">
              <div class="mt-2">Speed: {{ timeInterval }}ms </div>
              <b-form-input id="range-2" v-model="timeInterval" type="range" min="1" max="100"></b-form-input>
            </div> */
#slider-selectors {
  margin-bottom: 10px;
}

#columns-selector {
  width: 70%;
}

#selectors-spacing {
  width: 10%;
}

#speed-selectors {
  width: 20%;
}

#slider-selectors {
  display: flex;
}

#speed-selector {
  margin: 0px 0;
}

#action-btns {
  margin-top: 20px;
  display: flex;
}

.myColumns {
  width: 100%;
  display: flex;
}

#btn-space {
  width: 10%;
}


#sort-btn {
  width: 60%;
}

#reset-btn {
  width: 30%;
}

.home {
  /* margin-top: 50px; */
}

#selectors-container {
  color: #BEBEBE;
  background: black;
  /* margin: 0px auto 50px auto; */
  max-width: 400px;
  margin: 0px auto;
  padding: 20px;
}

#selectors {
  background: black;
  padding-bottom: 50px;
}

#left-selectors {
  /* background: green; */
  /* max-width: 400px; */
  padding: 10px;
  width: 50%;
}
#right-selectors {
  text-align: right;
  width: 50%;
  /* padding: 10px; */
  /* background: white; */
}
.selected {
  background: red !important;
}

#home-container {
  /* max-width: 800px; */
  /* margin: 0px auto; */
}

.column-container {
  max-width: 800px;
  margin: 0px auto;
}

.column {
  bottom: 0px;
  background: black;
}
</style>
