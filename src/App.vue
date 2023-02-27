<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/login_logo1.png" class="imgs">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #3b5e80;
  margin-top: 60px;
  min-width: 400px;
}
.imgs {
  width: 400px;
  margin: 0 auto;
}
</style>
