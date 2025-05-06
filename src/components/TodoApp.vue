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

const cancelActivity = (index) => {
  activities.value.splice(index, 1);
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

<ul class="activity-list">
  <li v-for="(activity, index) in activities" :key="index" class="activity-item">
    <button @click="cancelActivity(index)" class="delete-button small">Hapus</button>
    <span class="activity-text">{{ activity.text }}</span>
  </li>
</ul>
</template>

<style scoped>
</style>