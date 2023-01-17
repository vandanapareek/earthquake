<template>
  <HeaderComp/>
  <WorldMap/>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue'
import WorldMap from './components/WorldMap.vue'

export default {
  name: 'App',
  components: {
    HeaderComp,
    WorldMap
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
  margin: 20px;
}

.header {
  background-color: #3d5e80;
  color: #fff;
    font-size: 14px;
    font-weight: 300;
    display: block;
}
.header h1 {
  margin-bottom: 40px;
}
</style>
