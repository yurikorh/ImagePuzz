<template>
  <div class="add-text-dialog">
    <div>
      <label for="textContent">Text:</label>
      <input type="text" id="textContent" v-model="textContent">
    </div>
    <div>
      <label for="textSize">Size:</label>
      <input type="number" id="textSize" v-model="textSize">
    </div>
    <div>
      <label for="textColor">Color:</label>
      <input type="color" id="textColor" v-model="textColor">
    </div>
    <div>
      <label for="textFont">Font:</label>
      <input type="text" id="textFont" v-model="textFont">
    </div>
    <button @click="addText">Add Text</button>
  </div>
</template>

<script>
export default {
  name: 'AddTextDialog',
  data() {
    return {
      textContent: '',
      textSize: 20,
      textColor: '#000000',
      textFont: 'Arial',
    };
  },
  methods: {
    addText() {
      this.$emit('add-text', {
        content: this.textContent,
        size: this.textSize,
        color: this.textColor,
        font: this.textFont,
      });
    },
  },
};
</script>

<style scoped>
.add-text-dialog {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px;
  text-align: center;
  text-decoration: none;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #45a049;
}
</style>
