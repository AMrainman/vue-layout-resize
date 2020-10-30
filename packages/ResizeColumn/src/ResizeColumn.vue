<template>
  <div
    class="resize-column"
    ref="twoColumn"
  >
    <div
      class="left-column"
      :style="{width: `${bodyWidth}px`}"
    >
      <slot name="left"></slot>

      <div
        class="drag-bar"
        @mousedown="stickDown($event)"
      >
        <div
          class="drag-ico-original"
          :class="{ dragging: stickDrag}"
          v-if="!$slots.dragIco"
          @mousedown="stickDown($event)"
        ></div>
        <slot name="dragIco"></slot>
      </div>
    </div>

    <div class="right-column">
      <slot name="right"></slot>
    </div>
  </div>
</template>

<script>
import { throttle } from 'lodash'
export default {
  props: {
    leftInitW: {
      type: String,
      default: '300'
    },
    leftMinW: {
      type: String,
      default: '0'
    },
    rightMinW: {
      type: String,
      default: '0'
    }
  },
  data() {
    return {
      bodyWidth: Number(this.leftInitW),
      stickDrag: false
    }
  },
  computed: {
  },
  mounted() {
    document.documentElement.addEventListener('mousemove', this.move)
    document.documentElement.addEventListener('mouseup', this.up)
  },
  beforeDestroy() {
    document.documentElement.removeEventListener('mousemove', this.move)
    document.documentElement.removeEventListener('mouseup', this.up)
  },
  methods: {
    stickDown(ev) {
      ev.stopPropagation()

      this.stickDrag = true
      this.stickStartX = ev.pageX
      this.bodyOldWidth = this.bodyWidth
    },

    move: throttle(function(ev) {
      if (!this.stickDrag) return

      const offsetWidth = ev.pageX - this.stickStartX
      const width = offsetWidth + this.bodyOldWidth

      if (width < Number(this.leftMinW)) {
        this.bodyWidth = Number(this.leftMinW)
      } else if (width + Number(this.rightMinW) > this.$refs.twoColumn.clientWidth) {
        this.bodyWidth = this.$refs.twoColumn.clientWidth - Number(this.rightMinW)
      } else {
        this.bodyWidth = width
      }
    }, 50),

    up(ev) {
      this.stickDrag = false
    }
  }
}
</script>

<style lang='scss' scoped>
.resize-column {
  display: flex;
  height: 100%;
  .left-column {
    position: relative;

    .drag-bar:hover .drag-ico-original, .dragging {
      opacity: 1;
    }
  }
  .right-column {
    flex: 1;
  }
  .drag-bar {
    position: absolute;
    right: -5px;
    top: 0;
    bottom: 0;
    z-index: 999;
    width: 11px;
    cursor: col-resize;
    user-select: none;
  }
  .drag-ico-original {
    position: absolute;
    top: 20px;
    right: 0;
    z-index: 999;
    width: 9px;
    height: 9px;
    border: 1px solid #6c6c6c;
    background: #fff;
    opacity: 0;
    transition: opacity 0.2s ease;
  }
}
</style>
