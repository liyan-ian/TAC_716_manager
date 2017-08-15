<template>
  <div>
    <v-manager></v-manager>
  </div>
</template>

<script>
import manager from './components/manager/manager.vue';
export default {
  components: {
    'v-manager': manager
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
