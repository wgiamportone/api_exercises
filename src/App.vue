<template>
  <div id="app">
    <SpotFeed/>
    <InstaFeed/>
  </div>
</template>

<script>
import InstaFeed from './components/InstaFeed.vue'
import SpotFeed from './components/SpotFeed.vue'

export default {
  name: 'app',
  components: {
    InstaFeed,
    SpotFeed
  }
}
</script>

<style>
body {
  font: 1em/1.25 "Helvetica"
}
</style>
