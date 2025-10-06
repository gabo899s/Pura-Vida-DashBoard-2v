<template>
  <div id="app">
    <VisualBuilder />
  </div>
</template>

<script>
import VisualBuilder from './components/admin/VisualBuilder/VisualBuilder.vue'

export default {
  name: 'App',
  components: {
    VisualBuilder
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 100vw;
  height: 100vh;
  background: #F6EFD4;
  font-family: Arial, sans-serif;
}
</style>
