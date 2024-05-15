<template>
  <div class="canvas-area">
    <canvas id="myCanvas"></canvas>
  </div>
</template>

<script>
import { onMounted } from 'vue';

export default {
  name: 'CanvasArea',
  props: ['canvas', 'initCanvas', 'addPictures', 'addText', 'configureElement', 'removeElement', 'exportCanvas'],
  setup(props) {
    onMounted(() => {
      props.initCanvas();
    });
  }
};
</script>

<style scoped>
.canvas-area {
  border: 1px solid #ccc;
  border-radius: 10px;
  width: 100%;
  height: auto;
}
</style>
