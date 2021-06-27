<template>
  <div id="app">
    <HelloWorld msg="zpy"/>
    <Zone mse="zone"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Zone from './components/Zone.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    Zone
  }
}
</script>

<style>

</style>
