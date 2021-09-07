<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
    }
  },
  created() {
  }
}
</script>

<style>
html,
body {
  height: 100%;
  overflow: hidden;
  margin: 0px;
  background-color: #ffffff;
  font-family: Helvetica;
  position: relative;
}
* {
  /* touch-action: pan-y pan-x; */
  touch-action:none;
  touch-action:pan-y;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  margin: 0;
  padding: 0;
}
#app {
  width: 100%;
  height: 100%;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
