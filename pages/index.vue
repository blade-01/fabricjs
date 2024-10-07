<template>
  <div>
    <canvas id="fabricCanvas" width="800" height="600"></canvas>
  </div>
</template>

<script lang="ts" setup>
import * as fabric from "fabric";

const fabricCanvas = ref<fabric.Canvas | null>(null);

onMounted(() => {
  // Initialize Fabric.js canvas
  fabricCanvas.value = new fabric.Canvas("fabricCanvas", {
    width: 800,
    height: 600,
    backgroundColor: "#f0f0f0"
  });

  // Example of adding a text element
  const textbox = new fabric.IText("Draggable Text", {
    left: 100,
    top: 100,
    width: 150,
    fontSize: 30,
    fill: "#333",
    editable: true, // Enable text editing
    hasControls: true, // Allow resizing and rotating
    lockMovementX: false, // Allow free movement
    lockMovementY: false,
    borderColor: "blue", // Optional: border color when selected
    cornerColor: "red", // Optional: corner control points color
    cornerSize: 10 // Size of the control points for resizing
  });

  fabricCanvas.value.add(textbox);

  // Ensure canvas renders
  fabricCanvas.value.renderAll();

  // Enable dragging and movement
  // text.set({
  //   editable: true,
  //   hasControls: true,
  //   lockMovementX: false,
  //   lockMovementY: false
  // });
  // textbox.on("mousedown", () => {
  //   fabricCanvas.value!.setActiveObject(textbox);
  // });

  // Debugging logs
  console.log("Textbox added:", textbox);

  // Trigger text editing on double-click
  textbox.on("dblclick", () => {
    textbox.enterEditing();
    textbox.selectAll();
  });

  // Optional: Log when editing starts
  textbox.on("editing:entered", () => {
    console.log("Editing mode activated");
  });

  // Optional: Log when editing stops
  textbox.on("editing:exited", () => {
    console.log("Editing mode deactivated");
  });
});
</script>

<style>
canvas {
  border: 1px solid #000;
  user-select: none !important;
  pointer-events: auto !important;
}
</style>
