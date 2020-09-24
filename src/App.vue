<template>
  <div id="app">
    <video autoplay muted loop id="myVideo">
      <source src="../src/assets/DJ Sona Animated Splash Preview.mp4" type="video/mp4" />
    </video>

    <music />
  </div>
</template>

<script>
import music from "./components/music.vue";

export default {
  name: "App",
  components: {
    music,
  },
};
</script>

<style>
#myVideo {
  object-fit: cover;
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;

}
</style>
