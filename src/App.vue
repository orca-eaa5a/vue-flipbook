<template>
  <FlipBook 
    url="https://raw.githubusercontent.com/mozilla/pdf.js/master/web/compressed.tracemonkey-pldi-09.pdf"

  ></FlipBook>
</template>

<script>
import FlipBook from './components/FlipBook.vue';
export default {
  name: 'App',
  components: {
    FlipBook
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
