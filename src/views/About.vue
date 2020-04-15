<template>
  <div>
    <div>
      <button @click="addLine">新建线段</button>
    </div>
    <div @click="lineStart" ref="svgBox" class="svg-box">
      <svg height="100%" width="100%" version="1.1" xmlns="http://www.w3.org/2000/svg">
        <line x1="0" y1="0" x2="10" y2="10" style="stroke:rgb(99,99,99);stroke-width:2" />
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: []
    };
  },
  mounted() {
    this.start = true;
  },
  methods: {
    addLine() {},
    lineStart(e) {
      if (this.start) {
        const dom = this.$refs.svgBox;
        const line = dom.querySelector("line");
        line.setAttribute("x1", e.x - dom.offsetLeft); // 实际可能需要递归计算offsetTop
        line.setAttribute("y1", e.y - dom.offsetTop); // 用dom api是因为vue有延迟
        this.mousemove = function(e) {
          line.setAttribute("x2", e.x - dom.offsetLeft);
          line.setAttribute("y2", e.y - dom.offsetTop);
        };
        document.body.addEventListener("mousemove", this.mousemove);
      } else {
        document.body.removeEventListener("mousemove", this.mousemove);
      }
      this.start = !this.start;
    }
  }
};
</script>

<style>
.svg-box {
  height: 100vh;
}
</style>