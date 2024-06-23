<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Hi, <input type="text" id="name" placeholder="Your Name" v-model="nameData">
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="addTodo" class="todo-form">
        <div class="form-group">
          <label for="content">New Task</label>
          <input type="text" name="content" id="content" placeholder="Enter your task" v-model="input_content">
        </div>
        
        <div class="form-group">
          <label>Category</label>
          <div class="category-options">
            <label>
              <input type="radio" name="category" value="personal" v-model="input_category">
              <span class="category-label personal">Personal</span>
            </label>
            <label>
              <input type="radio" name="category" value="college" v-model="input_category">
              <span class="category-label college">College</span>
            </label>
          </div>
        </div>

        <button type="submit" class="btn-add-todo">Add Todo</button>
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list" id="todo-list">
        <div v-for="todo in todos" :key="todo.createdAt" class="todo-item" :class="{ 'done': todo.done }">
          <div class="todo-details">
            <input type="checkbox" v-model="todo.done" class="todo-checkbox">
            <span class="todo-content">{{ todo.content }}</span>
          </div>
          <button @click="removeTodo(todo)" class="btn-delete">Delete</button>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const todosData = ref([]);
const nameData = ref('');
const input_content = ref('');
const input_category = ref(null);

const todos = computed(() => todosData.value.sort((a, b) => b.createdAt - a.createdAt));

watch(nameData, (newVal) => {
  localStorage.setItem('name', newVal);
});

watch(todosData, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal));
}, { deep: true });

const addTodo = () => {
  if (!input_content.value.trim() || !input_category.value) return;
  todosData.value.push({
    content: input_content.value.trim(),
    category: input_category.value,
    done: false,
    createdAt: Date.now(),
  });
  input_content.value = '';
  input_category.value = null;
};

const removeTodo = (todo) => {
  todosData.value = todosData.value.filter(t => t !== todo);
};

onMounted(() => {
  nameData.value = localStorage.getItem('name') || '';
  todosData.value = JSON.parse(localStorage.getItem('todos')) || [];
});
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 20px;
}

.greeting {
  text-align: center;
}

.title {
  font-size: 24px;
  margin-bottom: 10px;
}

.create-todo,
.todo-list {
  width: 100%;
  max-width: 600px;
}

.todo-form {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.category-options {
  display: flex;
  gap: 10px;
  align-items: center;
}

.category-label {
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.category-label.personal {
  background-color: #007bff;
  color: #fff;
}

.category-label.college {
  background-color: #ffc107;
  color: #000;
}

.btn-add-todo {
  background-color: #4caf50;
  color: #fff;
  border: none;
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-add-todo:hover {
  background-color: #45a049;
}

.todo-list {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: #fff;
}

.todo-item.done {
  background-color: #d4edda;
}

.todo-details {
  display: flex;
  align-items: center;
}

.todo-checkbox {
  margin-right: 10px;
  cursor: pointer;
}

.todo-content {
  flex: 1;
  font-size: 16px;
}

.btn-delete {
  background-color: #dc3545;
  color: #fff;
  border: none;
  padding: 8px 12px;
  font-size: 14px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-delete:hover {
  background-color: #c82333;
}
</style>
