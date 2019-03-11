<template>
  <li v-if="checkTag()">
    <img :src="data.images.thumbnail.url" />
  </li>
</template>

<script>
export default {
  name: 'InstaItem',
  props:['data', 'activeTag'],
  methods: {
    checkTag () {
      if (!this.activeTag) {
        return true
      }
      return this.data.tags.indexOf(this.activeTag) > -1
    }
  }
}
</script>

<style scoped>
</style>
