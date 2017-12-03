<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: #E9EEF3;
  flex: 1;
  display: flex;
}
html, body {
  margin: 0;
  display: flex;
  flex: 1;
  height: 100%;
}
input:focus, textarea:focus {
  outline: none;
}
</style>
