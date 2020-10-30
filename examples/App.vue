<template>
  <div id="app">
    <ResizeColumn>
      <div slot="left" class="left">left</div>
      <div slot="right" class="right">right</div>
    </ResizeColumn>
    <ResizeRow class="rowLayout">
      <div slot="top" class="top">top</div>
      <div slot="bottom" class="bottom">bottom</div>
    </ResizeRow>
  </div>
</template>

<script>
import { ResizeColumn } from '../packages'
import { ResizeRow } from '../packages'

export default {
  components: {
    ResizeColumn,
    ResizeRow
  },
  data() {
    return {
    }
  },
  methods: {
  }
}
</script>
<style lang="scss" scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.left {
  height: 300px;
  border-right: 1px solid #666;
}

.rowLayout {
  height: 500px;
  .bottom {
    border-top: 1px solid #666;
  }
}

textarea {
  width: 300px;
  height: 50px;
  border: none;
  margin-top: 10px;
  background: cyan;
  font-family: fantasy;
  resize: none;
  &:focus {
    outline: none;
  }
}

table {
  width: max-content;
  margin: 20px auto;
  border-collapse: collapse;
  text-align: left;

  th {
    background: #f5f7fa;
  }
  th, td {
    padding: 4px 15px;
    border: 1px solid #ebeef5;
  }
}
</style>
