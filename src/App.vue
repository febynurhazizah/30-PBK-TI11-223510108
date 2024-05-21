<template>
  <div>
    <nav>
      <ul>
        <li @click="selectedMenu = 'todos'">Todos</li>
        <li @click="selectedMenu = 'posts'">Posts</li>
      </ul>
    </nav>
    <div>
      <Todos v-if="selectedMenu === 'todos'"
             :page-title="pageTitle"
             :table-title="tableTitle"
             :author="author"
             :activities="activities"
             @activity-added="handleActivityAdded"
             @activity-deleted="handleActivityDeleted" />
      <users v-if="selectedMenu === 'posts'" />
    </div>
  </div>
</template>

<script>
import Todos from './components/todos.vue';
import Users from './components/users.vue';

export default {
  components: {
    Todos,
    Users,
  },
  data() {
    return {
      selectedMenu: 'Todos',
      pageTitle: "Feby Nurhazizah - 223510108",
      tableTitle: "Tabel Kegiatan",
      author: "Feby Nurhazizah",
      activities: [
      { id: 1, name: 'Olahraga', completed: false },
      { id: 2, name: 'Nugas', completed: false },
      ],
    };
  },
  methods: {
    handleActivityAdded(newActivity) {
      this.activities.push(newActivity);
    },
    handleActivityDeleted(activityId) {
      const index = this.activities.findIndex(activity => activity.id === activityId);
      if (index !== -1) {
        this.activities.splice(index, 1);
      }
    },
    handleUserSelected(selectedUserId) {
      console.log('Selected user:', selectedUserId);

    },
    handlePostsFetched(posts) {
      console.log('Fetched posts:', posts);

    }
  }
};
</script>



<style scoped>
.background {
  background-image: url(image/1.jpg);
  color: white;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  min-height: 100vh;
  padding: 20px;
}
nav {
  background-color: grey;
  padding: 15px;
  color: white;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}
</style>