<template>
  <div
    class="resize-row"
    ref="twoRow"
  >
    <div
      class="top-column"
      :style="{height: `${bodyHeight}px`}"
    >
      <slot name="top"></slot>

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

    <div class="bottom-column">
      <slot name="bottom"></slot>
    </div>
  </div>
</template>

<script>
import { throttle } from 'lodash'
export default {
  props: {
    topInitH: {
      type: String,
      default: '300'
    },
    topMinH: {
      type: String,
      default: '0'
    },
    bottomMinH: {
      type: String,
      default: '0'
    }
  },
  data() {
    return {
      bodyHeight: Number(this.topInitH),
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
      this.stickStartY = ev.pageY
      this.bodyOldHeight = this.bodyHeight
    },

    move: throttle(function(ev) {
      if (!this.stickDrag) return

      const offsetHeight = ev.pageY - this.stickStartY
      const height = offsetHeight + this.bodyOldHeight

      if (height < Number(this.topMinH)) {
        this.bodyHeight = Number(this.topMinH)
      } else if (height + Number(this.bottomMinH) > this.$refs.twoRow.clientHeight) {
        this.bodyHeight = this.$refs.twoRow.clientHeight - Number(this.bottomMinH)
      } else {
        this.bodyHeight = height
      }
    }, 50),

    up(ev) {
      this.stickDrag = false
    }
  }
}
</script>

<style lang='scss' scoped>
.resize-row {
  display: flex;
  flex-flow: column nowrap;
  height: 100%;
  .top-column {
    position: relative;

    .drag-bar:hover .drag-ico-original, .dragging {
      opacity: 1;
    }
  }
  .bottom-column {
    flex: 1;
  }
  .drag-bar {
    position: absolute;
    left: 0;
    right: 0;
    bottom: -6px;
    z-index: 999;
    height: 11px;
    cursor: row-resize;
    user-select: none;
  }
  .drag-ico-original {
    position: absolute;
    top: 0;
    left: calc(50% - 5px);
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
