<template>
  <div id="app">
    <Controls
      @upload-background="handleUploadBackground"
      @upload-elements="handleUploadElements"
      @add-text="showAddTextDialog"
      @configure-element="handleConfigureElement"
      @remove-element="showRemoveDialog"
      @export-canvas="handleExportCanvas"
    />
    <div class="main-area">
      <UploadedPictures
        :pictures="uploadedPictures"
        @add-picture="handleAddPicture"
      />
      <CanvasArea
        :initCanvas="initCanvas"
        :canvas="canvas"
      />
    </div>
    <AddTextDialog
      v-if="showTextDialog"
      @add-text="handleAddText"
    />
    <ConfirmationDialog
      v-if="showRemoveConfirmation"
      @confirm-remove="handleRemoveElement"
      @cancel-remove="cancelRemove"
    />
  </div>
</template>

<script>
import CanvasArea from './components/CanvasArea.vue';
import Controls from './components/Controls.vue';
import UploadedPictures from './components/UploadedPictures.vue';
import AddTextDialog from './components/AddTextDialog.vue';
import ConfirmationDialog from './components/ConfirmationDialog.vue';
import { fabric } from 'fabric';

export default {
  name: 'App',
  components: {
    CanvasArea,
    Controls,
    UploadedPictures,
    AddTextDialog,
    ConfirmationDialog,
  },
  data() {
    return {
      canvas: null,
      uploadedPictures: [],
      showTextDialog: false,
      showRemoveConfirmation: false,
    };
  },
  methods: {
    initCanvas() {
      this.canvas = new fabric.Canvas('myCanvas');
      this.canvas.setDimensions({ width: window.innerWidth, height: window.innerHeight });
    },
    handleUploadBackground(event) {
      const reader = new FileReader();
      reader.onload = (e) => {
        fabric.Image.fromURL(e.target.result, (img) => {
          this.canvas.setDimensions({ width: img.width, height: img.height });
          this.canvas.setBackgroundImage(img, this.canvas.renderAll.bind(this.canvas), {
            scaleX: 1,
            scaleY: 1,
          });
        });
      };
      reader.readAsDataURL(event.target.files[0]);
    },
    handleUploadElements(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.uploadedPictures.push(e.target.result);
        };
        reader.readAsDataURL(files[i]);
      }
    },
    handleAddPicture(pic) {
      fabric.Image.fromURL(pic, (img) => {
        img.set({
          left: 100,
          top: 100,
          scaleX: 0.5,
          scaleY: 0.5,
          lockUniScaling: true,
        });
        this.canvas.add(img);
      });
    },
    showAddTextDialog() {
      this.showTextDialog = true;
    },
    handleAddText(textData) {
      const textElement = new fabric.Text(textData.content, {
        left: 100,
        top: 100,
        fontSize: textData.size,
        fill: textData.color,
        fontFamily: textData.font,
        lockUniScaling: true,
      });
      this.canvas.add(textElement);
      this.showTextDialog = false;
    },
    handleConfigureElement() {
      const activeObject = this.canvas.getActiveObject();
      if (activeObject) {
        const newColor = prompt("Enter new color (e.g., 'blue' or 'rgb(0, 0, 255)'):", activeObject.fill);
        activeObject.set('fill', newColor);
        this.canvas.renderAll();
      }
    },
    showRemoveDialog() {
      this.showRemoveConfirmation = true;
    },
    handleRemoveElement() {
      const activeObject = this.canvas.getActiveObject();
      if (activeObject) {
        this.canvas.remove(activeObject);
      }
      this.showRemoveConfirmation = false;
    },
    cancelRemove() {
      this.showRemoveConfirmation = false;
    },
    handleExportCanvas() {
      const dataURL = this.canvas.toDataURL({
        format: 'png',
        quality: 1.0,
      });
      const link = document.createElement('a');
      link.href = dataURL;
      link.download = 'canvas.png';
      link.click();
    },
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.main-area {
  display: flex;
  justify-content: space-between;
}

@media (max-width: 768px) {
  .main-area {
    flex-direction: column;
  }
}
</style>
