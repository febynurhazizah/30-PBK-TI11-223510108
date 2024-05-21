<template>
  <div class="background">
    <div class="container">
      <h1>Users</h1>
      <select v-model="selectedUser" class="select">
        <option v-for="user in users" :key="user.id" :value="user.id">
          {{ user.name }}
        </option>
      </select>
      <div v-if="isLoading" class="loading">Loading posts...</div>
      <div v-else class="posts-container">
        <h2>Posts</h2>
        <ul class="posts-list">
          <li v-for="post in posts" :key="post.id" class="post-title">{{ post.title }}</li>
        </ul>
      </div>
    </div>
  </div>
  <!-- Footer -->
  <footer>
    <p>&copy; 2099 Feby Nurhazizah</p>
  </footer>
</template>

<script setup>
import { ref, watch, defineEmits, defineProps } from 'vue';

const props = defineProps({
  users: {
    type: Array,
    required: true,
  },
});

const emits = defineEmits(['user-selected', 'posts-fetched']);

const selectedUser = ref(null);
const isLoading = ref(false);
const users = ref(props.users);
const posts = ref([]);

const getUsers = async () => {
  const resource = "https://jsonplaceholder.typicode.com/users";
  const response = await fetch(resource);
  users.value = await response.json();
};

const getPosts = async () => {
  if (selectedUser.value !== null) {
    posts.value = [];
    isLoading.value = true;
    const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
    posts.value = await response.json();
    isLoading.value = false;
    emits('posts-fetched', posts.value);
  }
};

getUsers();

watch(selectedUser, () => {
  getPosts();
  emits('user-selected', selectedUser.value);
});
</script>

<style scoped>
.background {
  background-image: url(image/3.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  box-sizing: border-box;
}

.container {
  background-color: rgba(255, 255, 255, 0.9);
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  width: 80%;
  max-width: 600px;
}

h1, h2 {
  margin: 10px 0;
  color: #333;
}

.select {
  padding: 10px;
  margin: 20px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 100%;
  box-sizing: border-box;
}

.posts-container {
  margin-top: 20px;
}

.posts-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.post-title {
  background-color: pink;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin: 5px 0;
  color: black;
}

.loading {
  color: #333;
}

footer {
  background-color: #333;
  color: white;
  text-align: center;
  padding: 10px 0;
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>
