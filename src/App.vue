<template>
  <div class="container">
    <header>
      <nav>
        <ul>
          <li>
            <router-link to="/posts" class="nav-link">Posts</router-link>
          </li>
          <li>
            <router-link to="/todos" class="nav-link">Todos</router-link>
          </li>
          <li>
            <router-link to="/albums" class="nav-link">Albums</router-link>
          </li>
        </ul>
      </nav>
    </header>
    <div class="content">
      <router-view />
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
  min-height: 100vh;
  background-color: #f0f0f0;
}

header {
  background-color: #333;
  padding: 10px;
  border-radius: 8px;
  width: 100%;
  max-width: 800px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

nav ul {
  list-style: none;
  display: flex;
  justify-content: center;
  padding: 0;
  margin: 0;
}

nav ul li {
  margin: 0 10px;
}

.nav-link {
  color: #fff;
  text-decoration: none;
  padding: 10px 20px;
  border-radius: 5px;
  transition: background-color 0.3s;
}

.nav-link:hover {
  background-color: #4CAF50;
}

.content {
  width: 100%;
  max-width: 800px;
  margin-top: 20px;
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
</style>
