<script setup>
import { ref } from 'vue';

const activities = ref([]);
const newActivity = ref('');

const addActivity = () => {
  if (newActivity.value.trim()) {
    activities.value.push({ text: newActivity.value, completed: false });
    newActivity.value = '';
  }
};
</script>

<template>
  <div class="page-wrapper">
    <div class="container">
      <h2 class="title">Daftar Tanaman</h2>
    </div>
  </div>

  <div class="input-container">
  <input v-model="newActivity" class="input-field" placeholder="Tambahkan tanaman baru" />
  <button @click="addActivity" class="add-button">Tambah</button>
</div>
</template>

<style scoped>
</style>