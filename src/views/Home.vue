<template>
  <div class="home">
    <div class="row">
      <div class="row-container">
        <div v-for="column in columns" v-bind:key="column">
          <div class="column" :style="divDimensions(10, column.length)">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'home',
  components: {
  },
  created() {
    for(var i = 0; i < this.numOfColumns; i++) {
      this.columns.push({ length: this.randomNumberBetween(100, 1000) })
    }
  },
  data() {
    return {
      numOfColumns: 100,
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
    }
  }
}
</script>

<style>

.row {
  max-width: 800px;
  margin: 0px auto;
  overflow: hidden;
}

.row-container {
  display: flex;
}

.column {
  background: blue;
  /* height: 100px; */
  /* width: 100px; */
}

</style>
