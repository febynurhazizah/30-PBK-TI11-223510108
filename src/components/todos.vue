<template>
  <div class="background">
    <h1>{{ pageTitle }}</h1>
    <p>{{ tableTitle }}</p> <br>

    <table>
      <thead>
        <tr>
          <th>Kegiatan</th>
          <th>Status</th>
          <th>Tindakan</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="activity in activities" :key="activity.id" :class="{ completed: activity.completed }">
          <td>
            <span :style="{ 'text-decoration': activity.completed ? 'line-through' : 'none', color: 'black' }">{{ activity.name }}</span>
          </td>
          <td>
            <span :style="{ color: 'black' }">{{ activity.completed ? 'Telah Selesai' : 'Belum Selesai' }}</span>
            <input type="checkbox" v-model="activity.completed">
          </td>
          <td>
            <button @click="cancelActivity(activity.id)">Hapus</button>
          </td>
        </tr>
      </tbody>
    </table>

    <form @submit.prevent="addActivity"> <br>
      <input type="text" v-model="newActivity" placeholder="Tambahkan Kegiatan"> <br>
      <button type="submit">Tambahkan</button>
    </form>
  </div>

  <!-- Footer -->
  <footer>
    <p>&copy; {{ author }}</p>
  </footer>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const pageTitle = 'Feby Nurhazizah - 223510108';
const tableTitle = 'Tabel Kegiatan';
const author = 'Feby Nurhazizah';
const activities = ref([
  { id: 1, name: 'Olahraga', completed: false },
  { id: 2, name: 'Nugas', completed: false },
]);
const newActivity = ref('');

const emits = defineEmits(['activity-added', 'activity-deleted']);

const addActivity = () => {
  if (newActivity.value.trim() !== '') {
    const activity = { id: Date.now(), name: newActivity.value, completed: false };
    activities.value.push(activity);
    emits('activity-added', activity);
    newActivity.value = '';
  }
};

const cancelActivity = (id) => {
  const index = activities.value.findIndex(activity => activity.id === id);
  if (index !== -1) {
    emits('activity-deleted', id);
    activities.value.splice(index, 1);
  }
};
</script>

<style scoped>
body {
  margin: 0;
  padding: 0;
}

.background {
  background-image: url(image/3.jpg);
  color: white;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  min-height: 100vh;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1, p {
  margin: 10px 0;
  color: black;
}

table {
  width: 100%;
  max-width: 800px;
  border-collapse: collapse;
  margin: 20px 0;
}

th, td {
  border: 1px solid white;
  padding: 12px;
  text-align: left;
  color: black; 
}

th {
  background-color: rgba(0, 0, 0, 0.7);
  color: white; 
}

td {
  background-color: rgba(255, 255, 255, 0.2);
}

.completed span {
  text-decoration: line-through;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input[type="text"] {
  width: 200px;
  padding: 10px;
  margin-bottom: 10px;
  border: none;
  border-radius: 4px;
}

button {
  background-color: white;
  color: black;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #e0e0e0;
}

footer {
  background-color: grey;
  color: white;
  text-align: center;
  padding: 10px 0;
  position: fixed;
  bottom: 0;
  width: 100%;
  opacity: 0.9;
}
</style>
