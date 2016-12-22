<template>
  <div id="app">
    <carousel></carousel>
  </div>
</template>

<script>
import Carousel from './components/Carousel';

export default {
  name: 'app',
  components: {
    Carousel,
  },
};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
