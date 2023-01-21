<template>
  <div class="img-container">
  </div>
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