<template>
  <div class="cvs_wrapper">
    <canvas id="cvs_home" width="300" height="300"></canvas>
  </div>
</template>

<script>
export default {
  name: "cvs",
  data() {
    return {
      aircraftList: [],
      xx: 300,
      yy: 0,
    }
  },
  mounted() {
    // this.init()
    this.$nextTick(() => {
      this.onloadCanvas()
    })
    // this.test()
  },
  methods: {
    init() {
      var sun = new Image();
      var moon = new Image();
      var earth = new Image();
      function init(){
        sun.src = 'https://mdn.mozillademos.org/files/1456/Canvas_sun.png';
        moon.src = 'https://mdn.mozillademos.org/files/1443/Canvas_moon.png';
        earth.src = 'https://mdn.mozillademos.org/files/1429/Canvas_earth.png';
        window.requestAnimationFrame(draw);
      }

      function draw() {
        var ctx = document.getElementById('cvs_home').getContext('2d');

        ctx.globalCompositeOperation = 'destination-over';
        ctx.clearRect(0,0,300,300); // clear canvas

        ctx.fillStyle = 'rgba(0,0,0,0.4)';
        ctx.strokeStyle = 'rgba(0,153,255,0.4)';
        ctx.save();
        ctx.translate(150,150);

        // Earth
        var time = new Date();
        ctx.rotate( ((2*Math.PI)/60)*time.getSeconds() + ((2*Math.PI)/60000)*time.getMilliseconds() );
        ctx.translate(105,0);
        ctx.fillRect(0,-12,50,24); // Shadow
        ctx.drawImage(earth,-12,-12);

        // Moon
        ctx.save();
        ctx.rotate( ((2*Math.PI)/6)*time.getSeconds() + ((2*Math.PI)/6000)*time.getMilliseconds() );
        ctx.translate(0,28.5);
        ctx.drawImage(moon,-3.5,-3.5);
        ctx.restore();

        ctx.restore();

        ctx.beginPath();
        ctx.arc(150,150,105,0,Math.PI*2,false); // Earth orbit
        ctx.stroke();

        ctx.drawImage(sun,0,0,300,300);

        window.requestAnimationFrame(draw);
      }

      init();
    },
    onloadCanvas() {
      this.img = new Image();
      this.img.onload = () => {
        this.timer()
        this.drawPlane()
      };
      this.img.src = require('../assets/300.png');
      // this.
    },
    drawPlane() {
      // 获取 canvas 容器
      let cvs = document.getElementById("cvs_home")
      let ctx = cvs.getContext("2d");
      // ctx.globalCompositeOperation = 'destination-over';
      ctx.clearRect(0,0,300,300); // clear canvas
      
      // this.yy += 10;
      ctx.drawImage(this.img, this.xx, this.yy, 50, 50);
      ctx.drawImage(this.img, this.xx-10, this.yy + 80, 100, 100);
      // ctx.translate(xx, yy)
      // ctx.rotate(((2*Math.PI)));
      window.requestAnimationFrame(this.drawPlane);
    },
    timer() {
      setInterval(() => {
        this.xx -= 1;
        if(this.xx <= -50) {
          this.xx = 300
        }
        // this.drawPlane()
      }, 1000/120)
    },
    test() {
      var ctx = document.getElementById('cvs_home').getContext('2d');
      var img = new Image();
      img.onload = function(){
        ctx.drawImage(img,0,0);
        ctx.beginPath();
        ctx.moveTo(30,96);
        ctx.lineTo(70,66);
        ctx.lineTo(103,76);
        ctx.lineTo(170,15);
        ctx.stroke();
      }
      img.src = 'https://mdn.mozillademos.org/files/5395/backdrop.png';
    }
  }
}
</script>