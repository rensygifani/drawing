<template>
  <div id="app" class="container">
    <h1 class="text-center">Draw a Digit</h1>
    <DrawingBoard />
  </div>
</template>

<script>
import DrawingBoard from './components/DrawingBoard.vue';

export default {
  name: 'App',
  components: {
    DrawingBoard,
  },
};
</script>

<style scoped>
/* Scoped styles for App.vue if needed */
</style>
