<template>
  <div>
    <h1>whiteboard</h1>
    <canvas ref="canvasRef" @mousedown="startDrawing" @mouseup="stopDrawing" @mousemove="draw" @mouseleave="stopDrawing"
      width="800" height="600">
    </canvas>
    <q-btn color="primary" @click="toggleMode">{{ mode === 'draw' ? 'Switch to Erase' : 'Switch to Draw'
      }}</q-btn>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';

const colorMode = useColorMode();

const canvasRef = ref(null);
const isDrawing = ref(false);
const mode = ref('draw');
const pageX = ref('n/a');
const pageY = ref('n/a');

// Toggle between different modes
const toggleMode = () => {
  mode.value = mode.value === 'draw' ? 'erase' : 'draw';
}

// Drawing/Erasing function 
const startDrawing = (event: MouseEvent) => {
  isDrawing.value = true;
  const canvas = canvasRef.value;
  if (!canvas) return;
  const ctx = canvas.getContext('2d');

  if (mode.value === 'erase') {
    ctx.globalCompositeOperation = 'destination-out';
    ctx.lineWidth = 20;
  } else {
    ctx.globalCompositeOperation = 'source-over';
    ctx.lineWidth = 5;
    ctx.strokeStyle = "#FFF";
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
  updatePosition(event);
};

// Stop drawing/erasing
const stopDrawing = () => {
  isDrawing.value = false;
};

// 
const draw = (event: MouseEvent) => {
  if (!isDrawing.value) return;
  const canvas = canvasRef.value;
  if (!canvas) return;

  const ctx = canvas.getContext('2d');
  ctx.lineTo(event.offsetX, event.offsetY);
  ctx.stroke();

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
};

const updatePosition = (event: MouseEvent) => {
  pageX.value = event.pageX;
  pageY.value = event.pageY;
};

/* TODO Make a fucntion to change colors
const colorWheel = () => {

};
*/

/* TODO Make pre-made shapes

*/

</script>

<style>
.dark-mode body {
  background-color: #111011;
  color: #ebf4f1;
}
</style>
