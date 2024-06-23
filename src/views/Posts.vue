<template>
  <div class="posts-container">
    <h1>Posts</h1>
    <div class="select-user">
      <label for="user-select">Choose User:</label>
      <select id="user-select" v-model="selectedUser" @change="fetchPosts">
        <option value="" disabled>Select user</option>
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
    </div>
    <div v-if="posts.length > 0" class="post-list">
      <h2>User's Posts: {{ selectedUserName }}</h2>
      <div class="posts-table">
        <div class="post-item" v-for="post in posts" :key="post.id">
          <div class="post-content">
            <h3>{{ post.title }}</h3>
            <p>{{ post.body }}</p>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="loading">
      <p>Loading...</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const users = ref([]);
const selectedUser = ref(null);
const selectedUserName = ref('');
const posts = ref([]);

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users');
    users.value = await response.json();
  } catch (error) {
    console.error('Error fetching users:', error);
  }
};

const fetchPosts = async () => {
  if (selectedUser.value) {
    try {
      const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${selectedUser.value}`);
      posts.value = await response.json();

      const selectedUserObj = users.value.find(user => user.id === parseInt(selectedUser.value));
      if (selectedUserObj) {
        selectedUserName.value = selectedUserObj.name;
      }
    } catch (error) {
      console.error('Error fetching posts:', error);
    }
  }
};

onMounted(() => {
  fetchUsers();
});
</script>

<style scoped>
.posts-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.select-user {
  margin-bottom: 20px;
}

.select-user label {
  margin-right: 10px;
}

.select-user select {
  padding: 8px 12px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.post-list {
  margin-top: 20px;
}

.loading {
  margin-top: 20px;
  text-align: center;
}

.posts-table {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.post-item {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 15px;
}

.post-item:hover {
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.post-content {
  font-size: 16px;
}

.post-content h3 {
  margin-bottom: 10px;
  font-size: 18px;
  color: #007bff;
}

.post-content p {
  color: #333;
  line-height: 1.6;
}

</style>
