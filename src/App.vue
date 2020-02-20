<template>
  <div id="app">
    <BaseTimer />
    <!-- <PomodoroTimer /> -->
  </div>
</template>

<script>
import BaseTimer from "./components/BaseTimer";
// import PomodoroTimer from "./components/PomodoroTimer";

export default {
  name: "App",

  components: {
    BaseTimer
    // PomodoroTimer
  }
};
</script>

<style>
body {
  background-color: #fff;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
}
</style>
