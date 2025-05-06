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

const toggleComplete = (index) => {
  activities.value[index].completed = !activities.value[index].completed;
};

const editingIndex = ref(null);
const editedText = ref('');

const startEditing = (index) => {
  editingIndex.value = index;
  editedText.value = activities.value[index].text;
};

const saveEdit = (index) => {
  if (editedText.value.trim()) {
    activities.value[index].text = editedText.value;
    editingIndex.value = null;
  }
};

const cancelEdit = () => {
  editingIndex.value = null;
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
    <input type="checkbox" :checked="activity.completed" @change="toggleComplete(index)" />

    <template v-if="editingIndex === index">
        <input v-model="editedText" class="input-field small" />
        <button @click="saveEdit(index)" class="add-button small">Simpan</button>
        <button @click="cancelEdit" class="delete-button small">Batal</button>
    </template>

    <template v-else>
        <span class="activity-text">{{ activity.text }}</span>
        <div class="button-group">
              <button @click="startEditing(index)" class="add-button small">Edit</button>
              <button @click="cancelActivity(index)" class="delete-button small">Hapus</button>
        </div>
    </template>
  </li>
</ul>
</template>

<style scoped>

.input-field.small {
  flex-grow: 1;
  width: 50%;
}

.add-button.small, .delete-button.small {
  padding: 5px 10px;
  font-size: 0.85rem;
}

.button-group {
  display: flex;
  gap: 5px;
}
</style>