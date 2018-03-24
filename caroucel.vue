<template>
  <div @pointerup="stopDrag"  @pointermove="onDrag">
    <div class="wrapper" @pointerdown="startDrag" >
      <div class="group" :style="currStyle">
        <div class="item prev">prev</div>
        <div class="item curr">current</div>
        <div class="item next">next</div>
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
      target: 0
    };
  },
  computed: {
    currStyle() {
      if (!this.drag) {
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
        setTimeout(() => {
          this.target = 0;
        }, 500);
      }
      if (this.move < -100) {
        this.target = -1;
        setTimeout(() => {
          this.target = 0;
        }, 500);
      }
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
  left: 0%;
}
.curr {
  background: #afa;
  left: 100%;
}
.next {
  background: #aaf;
  left: 200%;
}
</style>
