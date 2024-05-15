<template>
  <div class="controls">
    <input type="file" id="uploadBackground" @change="uploadBackground" accept="image/*" />
    <div v-if="backgroundUploaded">
      <input type="file" id="uploadElements" @change="uploadElements" accept="image/*" multiple />
      <button @click="addText">Add Text</button>
      <button @click="configureElement">Configure Element</button>
      <button @click="removeElement">Remove Element</button>
      <button @click="exportCanvas">Export Canvas</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Controls',
  data() {
    return {
      backgroundUploaded: false,
    };
  },
  methods: {
    uploadBackground(event) {
      this.$emit('upload-background', event);
      this.backgroundUploaded = true;
    },
    uploadElements(event) {
      this.$emit('upload-elements', event);
    },
    addText() {
      this.$emit('add-text');
    },
    configureElement() {
      this.$emit('configure-element');
    },
    removeElement() {
      this.$emit('remove-element');
    },
    exportCanvas() {
      this.$emit('export-canvas');
    },
  },
};
</script>

<style scoped>
.controls {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #45a049;
}
</style>
