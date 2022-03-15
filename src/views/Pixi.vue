<template>
  <div class="pixi" id="myPixi">
    <h1>pixi demo</h1>
    <button @click="toStart">toStart</button>
    <button @click="end">end</button>
  </div>
</template>

<script>
import * as PIXI from "pixi.js";
export default {
  name: "pixi",
  data() {
    return {
      opt: {
        width: 500,
        height: 500,
        backgroundAlpha: 0,
        myLoader: null,
        pixiHander: null,
        mask: null,
        maskFilter: null,
        idx: 0,
        speed: 1,
      },
    };
  },
  created() {
    this.pixiHander = new PIXI.Application(this.opt);
    this.myLoader = new PIXI.Loader();
    this.myLoader.add("view", require("../assets/view.jpg")).add("mask", require("../assets/mask.png")).load(this.loderImg);
  },
  mounted() {
    document.getElementById("myPixi").appendChild(this.pixiHander.view);
  },
  methods: {
    loderImg(loader, resources) {
      let view = new PIXI.Sprite(resources["view"].texture);
      this.mask = new PIXI.Sprite(resources["mask"].texture);
      this.mask.texture.baseTexture.wrapMode = PIXI.WRAP_MODES.REPEAT;
      this.maskFilter = new PIXI.filters.DisplacementFilter(this.mask);
      this.pixiHander.stage.addChild(view);
      this.pixiHander.stage.addChild(this.mask);
    },
    start(){
      this.idx = requestAnimationFrame(this.start);
      console.log(this.idx)
      this.mask.position.set(this.mask.x+=1, this.mask.y+=1);
      console.log(this.mask.x, this.mask.y)
    },
    toStart() {
      if(this.idx) return
      this.pixiHander.stage.filters = [this.maskFilter];
      this.start()
    },
    end() {
      this.pixiHander.stage.filters = [];
      cancelAnimationFrame(this.idx);
      this.idx = 0;
    }
  },
};
</script>