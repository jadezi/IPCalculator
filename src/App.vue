<template>
  <div id="app">
    <IP></IP>
  </div>
</template>

<script>
import IP from './components/IP.vue'

export default {
  name: 'app',
  components: {
    IP
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
