<template>
  <div @pointerup="stopDrag"  @pointermove="onDrag">
    <div class="wrapper" @pointerdown="startDrag" >
      <div class="item prev" :style="currStyle"></div>
      <div class="item curr" :style="currStyle"></div>
      <div class="item next" :style="currStyle"></div>
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
      cx: 0
    };
  },
  computed: {
    currStyle() {
      if (!this.drag) {
        return {
          transition: "all 0.5s"
        };
      }
      return {
        transform: `translate(${this.move}px, 0)`
      };
    }
  },
  methods: {
    startDrag(e) {
      var target_rect = e.currentTarget.getBoundingClientRect();
      var x = e.clientX - target_rect.left;
      this.drag = true;
      this.offset = x;
      // this.offset = e.offsetX;
    },
    onDrag(e) {
      if (this.drag) {
        var target_rect = e.currentTarget.getBoundingClientRect();
        var x = e.clientX - target_rect.left;
        this.move = x - this.offset;
        // this.offset = e.offsetX;
        // this.cx = this.cx + this.move;
        // console.log("ondrag");
      }
    },
    stopDrag(e) {
      this.drag = false;
      this.offset = 0;
      this.move = 0;
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

.prev {
  background: #faa;
  left: -100%;
}
.curr {
  background: #afa;
}
.next {
  background: #aaf;
  left: 100%;
}
</style>
