<template>
  <div id="app">
    <item/>
  </div>
</template>

<script>
import item from './components/item.vue'

export default {
  name: 'app',
  components: {
    item
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
}
html,body,#app{
  width: 100%;
  height: 100%;
}
</style>
