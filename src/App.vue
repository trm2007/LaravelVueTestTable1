<template>
  <div id="app" class="container">
    <simple-table />
  </div>
</template>

<script>
import SimpleTable from './components/SimpleTable.vue'

export default {
  name: 'App',
  components: {
    SimpleTable
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
