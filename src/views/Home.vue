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
      <div class="row-container">
        <div v-for="column in columns">
          <div class="column" :style="divDimensions(widthOfColumns, column)"></div>
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
      columns: []
    };
  },
  methods: {
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
      this.columns = [100];
      for (var i = 0; i < this.numOfColumns; i++) {
        this.columns.push(this.randomNumberBetween(10, 500));
      }
    },
    sortColumns() {
      var columns = document.getElementsByClassName("row-container")[0]
                            .childNodes;
      for(var i = 0; i < 1000; i++) {
        setTimeout(
          () => {
          columns.forEach((column, index  ) => {
            if (index - 1 >= 0) {
              var prevSelectedCol = columns[index - 1]
            }
            var selectedCol = columns[index];
            var selectedColBackground = columns[index]
            setTimeout(
              () => {
                selectedCol = selectedCol.childNodes[0]
                if (prevSelectedCol) {
                  prevSelectedCol = prevSelectedCol.childNodes[0]
                  prevSelectedCol.parentNode.setAttribute('style', `background: ${'black'};`)
                  var prevSelectedColHeight = prevSelectedCol.style.height;
                  prevSelectedColHeight = prevSelectedColHeight.replace(/[px]/, '')
                  prevSelectedColHeight = parseInt(prevSelectedColHeight)
                }
    
                // selectedCol.parentNode.setAttribute('style', 'background: blue;')
                var selectedColHeight = selectedCol.style.height
                selectedColHeight = selectedColHeight.replace(/[px]/,'')
                selectedColHeight = parseInt(selectedColHeight)
    
                if(selectedColHeight > prevSelectedColHeight) {
                  prevSelectedCol.setAttribute('style', `width:${this.widthOfColumns}px; height: ${selectedColHeight}px;`)
                  selectedCol.setAttribute('style', `width:${this.widthOfColumns}px; height: ${prevSelectedColHeight}px;`)
                }
    
                if (index === this.numOfColumns - 1) {
                  setTimeout(() => {
                    selectedCol.parentNode.classList.remove("selected");
                    prevSelectedCol.parentNode.classList.remove("selected");
                  }, this.timeInterval);
                }
              },
              index * this.timeInterval,
              selectedCol,
              prevSelectedCol,
            );
          });
        }, i * 100);
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
    mySort(a, b) {
      return a - b;
    }
  }
};
</script>

<style>
.row {
  margin: 100px auto 0px auto;
  margin: 0px auto;
  width: 800px;
  background: black;
}

.row-container {
  display: flex;
}

.selected {
  background: blue !important;
}

.column {
  background: white;
}
</style>
