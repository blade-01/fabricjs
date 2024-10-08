<template>
  <div>
    <canvas id="fabricCanvas" ref="fabricCanvas"></canvas>
    <button @click="toggleEditMode">Toggle Edit Mode</button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import * as fabric from 'fabric';

const fabricCanvas = ref(null);
const editMode = ref(false);
let textbox;

onMounted(() => {
  // Initialize Fabric.js canvas
  fabricCanvas.value = new fabric.Canvas("fabricCanvas", {
    width: 800,
    height: 600,
    backgroundColor: "#f0f0f0"
  });

  // Create the text element
  textbox = new fabric.IText("Draggable Text", {
    left: 100,
    top: 100,
    width: 250,
    fontSize: 50,
    fill: "#333",
    borderColor: "blue",
    cornerColor: "red",
    cornerSize: 10,
    padding: 5,
    textAlign: "center",
  });

  fabricCanvas.value.add(textbox);
  fabricCanvas.value.setActiveObject(textbox);
  fabricCanvas.value.renderAll();

  // Double-click to edit
  fabricCanvas.value.on('mouse:dblclick', function(options) {
    if (options.target && options.target.type === 'i-text') {
      enterEditMode(options.target);
    }
  });

  // Click outside to exit edit mode
  fabricCanvas.value.on('mouse:down', function(options) {
    if (!options.target) {
      exitEditMode();
    }
  });

  // Ensure controls are visible after editing
  textbox.on("editing:exited", () => {
    console.log("Editing mode deactivated");
    exitEditMode();
  });
});

const enterEditMode = (target) => {
  editMode.value = true;
  target.enterEditing();
  target.selectAll();
  fabricCanvas.value.renderAll();
};

const exitEditMode = () => {
  editMode.value = false;
  if (textbox.isEditing) {
    textbox.exitEditing();
  }
  // textbox.set({
  //   editMode: true
  // })
  // fabricCanvas.value.setActiveObject(textbox);
  fabricCanvas.value.requestRenderAll();
};

const toggleEditMode = () => {
  if (editMode.value) {
    exitEditMode();
  } else {
    enterEditMode(textbox);
  }
};
</script>
