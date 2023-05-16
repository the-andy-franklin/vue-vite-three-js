<template>
  <div id="app">
    <SpinningCube/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SpinningCube from './components/SpinningCube.vue';

export default defineComponent({
  name: 'App',
  components: {
    SpinningCube
  }
});
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>