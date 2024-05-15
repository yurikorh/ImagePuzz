<template>
  <div class="confirmation-dialog">
    <p>Are you sure you want to remove this element?</p>
    <button @click="confirmRemove">Yes</button>
    <button @click="cancelRemove">No</button>
  </div>
</template>

<script>
export default {
  name: 'ConfirmationDialog',
  methods: {
    confirmRemove() {
      this.$emit('confirm-remove');
    },
    cancelRemove() {
      this.$emit('cancel-remove');
    },
  },
};
</script>

<style scoped>
.confirmation-dialog {
  display: flex;
  flex-direction: column;
  gap: 10px;
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

button {
  background-color: #f44336;
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
  background-color: #e53935;
}
</style>
