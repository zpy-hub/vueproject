<template>
  <div class="hello">
    <h5>{{ mse }}</h5>
    <testa totesta="testa"></testa>
  </div>
</template>

<script>
import testa from '../subcomponents/testa'

export default {
  name: 'Zone',
  components: {
    testa
  },
  props: {
    mse: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
