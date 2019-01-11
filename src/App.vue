<template>
  <div id="app">
    <header>
      <ul>
        <li>Home</li>

        <li>1</li>

        <li> 1</li>
      </ul>
    </header>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <test />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import test from './components/test.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    test
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
  margin-top: 1vw;
}

ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
</style>
