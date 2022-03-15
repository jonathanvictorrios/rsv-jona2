<template>
  <div id="app" class="mt-0">
    <HeaderRsv></HeaderRsv>
    <MapVehiculos></MapVehiculos>
    <hr>
    <FooterRsv></FooterRsv>
  </div>
</template>

<script>
import FooterRsv from './components/FooterRsv.vue'
import MapVehiculos from './components/MapVehiculos.vue'
import HeaderRsv from './components/HeaderRsv.vue'


export default {
  name: 'App',
  components: {
    MapVehiculos,
    FooterRsv,
    HeaderRsv
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
