<template>
  <div>
    <p>{{this.remainMin}}:{{this.remainSec}}</p>
    <canvas id="timerLine" width="300" height="300"></canvas>
  </div>
</template>

<script>
export default {
  props: ["remainMin", "remainSec"],
  data: function() {
    return {
      ctx: ""
    };
  },
  mounted: function() {
    const timerLine = document.getElementById("timerLine");
    this.ctx = timerLine.getContext("2d");
    let ctx = this.ctx;
    ctx.beginPath();
    ctx.lineWidth = 5;
    ctx.strokeStyle = "#F5E5B3";
    ctx.arc(70, 70, 60, 0 * Math.PI / 180, 360 * Math.PI / 180);
    ctx.stroke();
    ctx.closePath();

    ctx.lineWidth = 6;
    ctx.strokeStyle = "#1D1C22";

    ctx.beginPath();
    let remainAngle = (60 - this.remainMin) * 6 - 90;
    ctx.arc(70, 70, 60, -90 * Math.PI / 180, remainAngle * Math.PI / 180);
    ctx.stroke();
    ctx.closePath();
  },
  updated: function() {
    if (this.remainMin == 60) {
      this.initTimer();
      return;
    }
    let ctx = this.ctx;
    ctx.beginPath();
    let remainAngle = (60 - this.remainMin) * 6 - 90;
    ctx.arc(70, 70, 60, -90 * Math.PI / 180, remainAngle * Math.PI / 180);
    ctx.stroke();
    ctx.closePath();
  },
  methods: {
    initTimer: function() {
      let ctx = this.ctx;
      ctx.beginPath();
      ctx.lineWidth = 5;
      ctx.strokeStyle = "#F5E5B3";
      ctx.arc(70, 70, 60, 0 * Math.PI / 180, 360 * Math.PI / 180);
      ctx.stroke();
      ctx.closePath();
    }
  }
};
</script>

<style>
</style>
