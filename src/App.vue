<template>
  <div id="app">
    <app-filelist></app-filelist>
  </div>
</template>

<script>
import FileList from './components/FileList.vue';

export default {
  name: 'App',
  components: {
    'app-filelist': FileList,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 62.5%;
  text-align: center;
  color: #2c3e50;
  margin-top: 6rem;
  text-align: left;
}
</style>
