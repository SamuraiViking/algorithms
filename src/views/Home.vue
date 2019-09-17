<template>
  <div class="home">
    <div class="row">
      <div v-for="column in columns" v-bind:key="column">
        <div class="column" :style="divDimensions(200, column.length)">
          {{ column.length }}
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
  data() {
    return {
      columns: [
        { length: 500 },
        { length: 100 },
        { length: 500 },
        { length: 200 },
        { length: 500 },
        { length: 300 },
      ]
    }
  },
  methods: {
    divDimensions(width, height) {
      return `width: ${width}px; height: ${height}px;`
    }
  }
}
</script>

<style>

.row {
  display: flex;
}

.column {
  background: blue;
  /* height: 100px; */
  /* width: 100px; */
}

</style>
