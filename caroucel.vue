<template>
  <div @pointerup="stopDrag"  @pointermove="onDrag">
    <div class="wrapper" @pointerdown="startDrag" >
      <div class="group" :style="currStyle">
        <div class="item prev" v-html="page(-1)"></div>
        <div class="item curr" v-html="page(0)"></div>
        <div class="item next" v-html="page(1)"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      drag: false,
      offset: 0,
      move: 0,
      target: 0,
      animate: false,
      pageIndex: 1,
      pages: [
        `<div class="page" style="background: #FAA;">Page 1</div>`,
        `<div class="page" style="background: #AFA;">Page 2</div>`,
        `<div class="page" style="background: #AAF;">Page 3</div>`,
        `<div class="page" style="background: #FFA;">Page 4</div>`,
        `<div class="page" style="background: #AFF;">Page 5</div>`
      ]
    };
  },
  computed: {
    currStyle() {
      if (this.animate) {
        return {
          transform: `translate(${this.target * 400 - 400}px, 0)`,
          transition: "all 0.5s"
        };
      }
      return {
        transform: `translate(${this.move - 400}px, 0)`
      };
    }
  },
  methods: {
    page(index) {
      return this.pages[this.pageIndex + index];
    },
    startDrag(e) {
      var target_rect = e.currentTarget.getBoundingClientRect();
      var x = e.clientX - target_rect.left;
      this.drag = true;
      this.offset = x;
    },
    onDrag(e) {
      if (this.drag) {
        var target_rect = e.currentTarget.getBoundingClientRect();
        var x = e.clientX - target_rect.left;
        this.move = x - this.offset;
        console.log(this.move);
      }
    },
    stopDrag() {
      if (this.move > 100) {
        this.target = 1;
        this.animate = true;
        setTimeout(() => {
          this.pageIndex--;
          this.target = 0;
          this.animate = false;
        }, 500);
      }
      if (this.move < -100) {
        this.target = -1;
        this.animate = true;
        setTimeout(() => {
          this.pageIndex++;
          this.target = 0;
          this.animate = false;
        }, 500);
      }
      this.drag = false;
      this.animate = true;
      this.offset = 0;
      this.move = 0;
      setTimeout(() => {
        this.animate = false;
      }, 500);
    }
  },
  mounted() {
    document.body.addEventListener("contextmenu", e => {
      console.log("hmm");
      e.preventDefault();
      return false;
    });
  }
};
</script>

<style>
.wrapper {
  overflow: hidden;
  background: #333;
  width: 400px;
  height: 300px;
  position: relative;
}
.item {
  width: 400px;
  height: 300px;
  position: absolute;
  user-select: none;
}
.page {
  border: 4px solid #333;
  height: 100%;
  background: white;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  font-weight: 900;
  color: #333;
}

.prev {
  left: 0%;
}
.curr {
  left: 100%;
}
.next {
  left: 200%;
}
</style>
