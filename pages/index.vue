<template>
  <div>
    <Toolbar>
      <template #start>
        <Button class="mr-2" type="button" icon="pi pi-ellipsis-h" @click="toggle" aria-haspopup="true"
          aria-controls="overlay_menu" />
        <Menu ref="menuRef" id="overlay_menu" :model="items" :popup="true" />
        <Button class="mr-2" @click="toggleMode">
          <i :class="[mode === 'draw' ? 'pi pi-pencil' : 'pi pi-eraser']"></i>
        </Button>
      </template>
      <template #center>
        <Button icon="pi pi-print" class="mr-2" />
        <Button icon="pi pi-info" class="mr-2" />
        <Button icon="pi pi-arrow-right" class="mr-2" />
        <Button icon="pi pi-arrows-alt" class="mr-2" />
      </template>
      <template #end>
        <ColorPicker v-model="colorHex" inputId="cp-hex" format="hex" class="mr-2" />
        <Button label="Share" icon="pi pi-check" :model="items"></Button>
      </template>
    </Toolbar>
    <canvas ref="canvasRef" @mousedown="startDrawing" @mouseup="stopDrawing" @mousemove="draw" @mouseleave="stopDrawing"
      width="800" height="600">
    </canvas>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref, reactive } from 'vue';
import ColorPicker from 'primevue/colorpicker';
import Toolbar from 'primevue/toolbar';
import Menu from 'primevue/menu';

const colorMode = useColorMode();

const canvasRef = ref(null);
const isDrawing = ref(false);
const mode = ref('draw');
const color = ref('#FFF');
const pageX = ref('n/a');
const pageY = ref('n/a');
const menuRef = ref(null);

const toggle = (event) => {
  menuRef.value.toggle(event); // This calls the toggle method of the Menu component
};
// Toggle between different modes
const toggleMode = () => {
  mode.value = mode.value === 'draw' ? 'erase' : 'draw';
}

// Assuming other setup code remains unchanged

const items = ref([
  { label: 'Item 1', icon: 'pi pi-fw pi-plus', command: () => { console.log('Item 1 clicked'); } },
  { label: 'Item 2', icon: 'pi pi-fw pi-download', command: () => { console.log('Item 2 clicked'); } },
  { label: 'Item 3', icon: 'pi pi-fw pi-refresh', command: () => { console.log('Item 3 clicked'); } }
]);

// The rest of your script setup...
// Drawing/Erasing function 
const startDrawing = (event: MouseEvent) => {
  isDrawing.value = true;
  const canvas = canvasRef.value;
  if (!canvas) return;
  const ctx = canvas.getContext('2d');

  console.log("Current color:", color.value);
  ctx.strokeStyle = color.value;

  if (mode.value === 'erase') {
    ctx.globalCompositeOperation = 'destination-out';
    ctx.lineWidth = 40;
  } else {
    ctx.globalCompositeOperation = 'source-over';
    ctx.lineWidth = 5;
    //    ctx.strokeStyle = "#FFF";
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

const adjustCanvasSize = () => {
  const canvas = canvasRef.value;
  if (canvas) {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
};

onMounted(() => {
  adjustCanvasSize();
  window.addEventListener('resize', adjustCanvasSize);
});

onUnmounted(() => {
  window.removeEventListener('resize', adjustCanvasSize);
});

</script>

<style>
canvas {
  position: fixed;
  /* Use fixed or absolute depending on your layout */
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: -1;
  /* Ensure it's behind other content, adjust as needed */
}

.dark-mode body {
  background-color: #111011;
  color: #ebf4f1;
}
</style>
