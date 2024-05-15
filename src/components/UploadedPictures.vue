<template>
  <div class="uploaded-pictures" :class="deviceClass">
    <button v-if="isFolded" @click="toggleFolded" class="toggle-button">▶</button>
    <div v-else>
      <button @click="toggleFolded" class="toggle-button">◀</button>
      <div class="pictures-container">
        <img v-for="(pic, index) in pictures" :key="index" :src="pic" @click="addPicture(pic)" class="picture" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UploadedPictures',
  data() {
    return {
      pictures: [],
      isFolded: true,
    };
  },
  computed: {
    deviceClass() {
      const width = window.innerWidth;
      if (width > 1024) return 'desktop';
      if (width > 768) return 'tablet';
      return 'phone';
    },
  },
  methods: {
    addPicture(pic) {
      this.$emit('add-picture', pic);
    },
    toggleFolded() {
      this.isFolded = !this.isFolded;
    },
  },
  mounted() {
    window.addEventListener('resize', this.updateDeviceClass);
  },
  unmounted() {
    window.removeEventListener('resize', this.updateDeviceClass);
  },
  updateDeviceClass() {
    this.deviceClass = this.getDeviceClass();
  },
};
</script>

<style scoped>
.uploaded-pictures {
  display: flex;
  flex-direction: column;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 10px;
}

.desktop {
  height: 100%;
}

.tablet {
  height: 100%;
}

.phone {
  position: fixed;
  bottom: 0;
  width: 100%;
}

.toggle-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px;
  text-align: center;
  cursor: pointer;
  border-radius: 5px;
}

.pictures-container {
  display: flex;
  flex-wrap: nowrap;
  overflow-x: auto;
}

.picture {
  margin: 5px;
  max-height: 100px;
  cursor: pointer;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.picture:hover {
  border-color: #4CAF50;
}
</style>
