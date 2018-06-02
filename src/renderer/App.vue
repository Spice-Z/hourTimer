<template>
  <div id="app">
    <timer></timer>
  </div>
</template>

<script>
import Timer from "@/components/Timer";

export default {
  name: "hour-timer",
  components: {
    Timer
  }
};
</script>

<style>
body {
  -webkit-app-region: drag;
  background: #323338;
  color: #f7f7f7;
}
</style>
