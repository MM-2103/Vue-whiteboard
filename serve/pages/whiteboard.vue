<template>
  <div>
    <h1 class="test-lg">whiteboa</h1>
    <canvas ref="canvasRef" @mousedown="startDrawing" @mouseup="stopDrawing" @mousemove="draw" @mouseleave="stopDrawing"
      width="800" height="600" class="border-dotted">
    </canvas>
    <button @click="toggleMode">{{ mode === 'draw' ? 'Switch to Erase' : 'Switch to Draw' }}</button>
  </div>
  <div class="box">
    <p><code>pageX</code>: <span id="x">{{ pageX }}</span></p>
    <p><code>pageY</code>: <span id="y">{{ pageY }}</span></p>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';

const canvasRef = ref(null);
const isDrawing = ref(false);
const mode = ref('draw');
const pageX = ref('n/a');
const pageY = ref('n/a');


const toggleMode = () => {
  mode.value = mode.value === 'draw' ? 'erase' : 'draw';
};

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
    ctx.lineWidth = 2;
  }

  ctx.beginPath();
  ctx.moveTo(event.offsetX, event.offsetY);
  updatePosition(event);
};

const stopDrawing = () => {
  isDrawing.value = false;
};

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

onMounted(() => {
  const canvas = canvasRef.value;
  if (canvas) {
    const ctx = canvas.getContext('2d');
    ctx.lineWidth = 5;
    ctx.lineCap = 'round';
  }
});

</script>
