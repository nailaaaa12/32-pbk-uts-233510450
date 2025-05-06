<script setup>
import { ref } from 'vue';

const activities = ref([]);
const newActivity = ref('');
const showOnlyIncomplete = ref(false);
const editingIndex = ref(null);
const editedText = ref('');

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

const filteredActivities = () => {
  return showOnlyIncomplete.value
    ? activities.value.filter(activity => !activity.completed)
    : activities.value;
};

const totalActivities = () => activities.value.length;
const incompleteActivities = () =>
  activities.value.filter(a => !a.completed).length;
</script>

<template>
  <div class="page-wrapper">
    <div class="container">
      <h2 class="title">Daftar Tanaman</h2>

      <div class="input-container">
        <input v-model="newActivity" class="input-field" placeholder="Tambahkan tanaman baru" />
        <button @click="addActivity" class="add-button">Tambah</button>
      </div>

      <label class="filter-label">
        <input type="checkbox" v-model="showOnlyIncomplete" />
        Tampilkan daftar tanaman yang belum dibeli
      </label>

      <div class="summary">
        <p>Total: {{ totalActivities() }} | Belum dibeli: {{ incompleteActivities() }}</p>
      </div>

      <ul class="activity-list">
        <li v-if="filteredActivities().length === 0" class="activity-item empty-item">
          Tidak ada tanaman untuk ditampilkan.
        </li>

        <li v-for="(activity, index) in filteredActivities()" :key="index" class="activity-item">
          <input type="checkbox" :checked="activity.completed" @change="toggleComplete(index)" />
          
          <template v-if="editingIndex === index">
            <input v-model="editedText" class="input-field small" />
            <button @click="saveEdit(index)" class="add-button small">Simpan</button>
            <button @click="cancelEdit" class="delete-button small">Batal</button>
          </template>

          <template v-else>
            <span class="activity-text" :class="{ completed: activity.completed }">
              {{ activity.text }}
            </span>
            <div class="button-group">
              <button @click="startEditing(index)" class="add-button small">Edit</button>
              <button @click="cancelActivity(index)" class="delete-button small">Hapus</button>
            </div>
          </template>
        </li>
      </ul>

      <!-- Google Fonts -->
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Cal+Sans&display=swap" rel="stylesheet">
    </div>
  </div>
</template>

<style scoped>
.page-wrapper {
  font-family: 'Cal Sans', sans-serif;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(to right, #745c74, #e2e2e2);
  padding: 20px;
}

.container {
  background: linear-gradient(to bottom right, #fdfbfb, #ebedee);
  padding: 24px;
  border-radius: 16px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
  max-width: 540px;
  width: 100%;
  color: #333;
}

.title {
  text-align: center;
  margin-bottom: 20px;
  font-weight: normal;
  font-size: 1.6rem;
  color: #333;
}

.input-container {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.input-field {
  flex-grow: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1.5px solid #bbb;
  background: #fff;
  color: #333;
  font-family: 'Cal Sans', sans-serif;
  font-size: 1rem;
}

.input-field.small {
  flex-grow: 0;
  width: 50%;
}

.add-button, .delete-button {
  background: linear-gradient(to right, #dba1fd, #f0c2fb);
  color: #333;
  cursor: pointer;
  padding: 8px 14px;
  border-radius: 8px;
  border: none;
  font-weight: normal;
  font-family: 'Cal Sans', sans-serif;
  font-size: 0.95rem;
  transition: all 0.2s ease;
}

.add-button:hover, .delete-button:hover {
  background: linear-gradient(to right, #674474, #c0b6c2);
}

.add-button.small, .delete-button.small {
  padding: 6px 10px;
  font-size: 0.85rem;
}

.filter-label {
  display: flex;
  align-items: center;
  margin-top: 15px;
  gap: 8px;
  font-weight: normal;
  color: #444;
}

.summary {
  margin-top: 10px;
  text-align: center;
  font-weight: normal;
  color: #555;
}

.activity-list {
  list-style: none;
  padding: 0;
  margin-top: 15px;
}

.activity-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: linear-gradient(to right, #eeddf6, #f4d9f7);
  padding: 12px;
  border-radius: 10px;
  margin-bottom: 10px;
  gap: 10px;
}

.activity-text {
  flex-grow: 1;
  font-weight: normal;
  font-family: 'Cal Sans', sans-serif;
  font-size: 1rem;
}

.activity-text.completed {
  text-decoration: line-through;
  color: #999;
}

.button-group {
  display: flex;
  gap: 6px;
}

.empty-item {
  justify-content: center;
  font-style: italic;
  font-weight: normal;
  color: #777;
}
</style>