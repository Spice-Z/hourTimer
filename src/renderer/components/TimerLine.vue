<template>
  <div class="timer">
    <p class="timer__time">{{this.remainMin}}:{{this.remainSec | doubleDigit }}</p>
    <canvas id="timerLine" width="1000" height="1000"></canvas>
  </div>
</template>

<script>
let imgSrc = ".~@/assets/pencil.png";
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
    ctx.lineWidth = 50;
    ctx.strokeStyle = "#F5E5B3";
    ctx.arc(500, 500, 450, 0 * Math.PI / 180, 360 * Math.PI / 180);
    ctx.stroke();
    ctx.closePath();
    let img = new Image();
    img.src = require("../assets/pencil.png");
    img.onload = function() {
      ctx.drawImage(img, 250, 250, 500, 500);
    };

    ctx.lineWidth = 51;
    ctx.strokeStyle = "#1D1C22";

    ctx.beginPath();
    let remainAngle = (60 - this.remainMin) * 6 - 90;
    ctx.arc(500, 500, 450, -90 * Math.PI / 180, remainAngle * Math.PI / 180);
    ctx.stroke();
    ctx.closePath();
  },
  updated: function() {
    if (this.remainMin == 60) {
      this.initTimer();
      return;
    }
    let ctx = this.ctx;
    ctx.strokeStyle = "#1D1C22";
    ctx.beginPath();
    let remainAngle = (60 - this.remainMin) * 6 - 90;
    ctx.arc(500, 500, 450, -90 * Math.PI / 180, remainAngle * Math.PI / 180);
    ctx.stroke();
    ctx.closePath();
  },
  methods: {
    initTimer: function() {
      let ctx = this.ctx;
      ctx.beginPath();
      ctx.lineWidth = 50;
      ctx.strokeStyle = "#F5E5B3";
      ctx.arc(500, 500, 450, 0 * Math.PI / 180, 360 * Math.PI / 180);
      ctx.stroke();
      ctx.closePath();
    }
  },
  filters: {
    doubleDigit: function(value) {
      if (value < 10) {
        return "0" + value;
      }
      return value;
    }
  }
};
</script>

<style>
.timer__time {
  text-align: center;
  font-size: 26px;
}
</style>
