<template>
  <div id="app">
    <BuySurge v-if="!isMobilePlatform"></BuySurge>
    <BuySurgeMobile v-if="isMobilePlatform"></BuySurgeMobile>
  </div>
</template>

<script>
import BuySurge from "./components/BuySurge";
import BuySurgeMobile from "./components/BuySurgeMobile";

export default {
  name: 'App',
  components: {
    BuySurge,
    BuySurgeMobile
  },
  data: function () {
    return {
      isMobilePlatform : false
    }
  },created() {
  if(/Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent)) {
    this.isMobilePlatform = true
  } else {
    this.isMobilePlatform = false
  }
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial,Ebrima, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #ffffff;
  margin-top: 0px;
  background-color: #1A1A1A;
}


</style>
