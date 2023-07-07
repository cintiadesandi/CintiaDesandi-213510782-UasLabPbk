<template>
  <div id="app">
    <header class="header">
      <h1 class="logo">Tia's Widget</h1>
      <nav>
        <ul>
          <li><router-link to="/" class="nav-link">Weather</router-link></li>
          <li><router-link to="/location" class="nav-link">Location</router-link></li>
          <li><router-link to="/stopwatch" class="nav-link">Stopwatch</router-link></li>
          <li><router-link to="/wordscramble" class="nav-link">Word Scramble</router-link></li>
          <li><router-link to="/photo" class="nav-link">Photo</router-link></li>
        </ul>
      </nav>
    </header>
    <div class="content">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
#app {
  text-align: center;
  min-height: 100vh;
  background: linear-gradient(to bottom, #E27DA1, #846C98, #356E9A);
}

.header {
  background: linear-gradient(to right, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.9));
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}


.logo {
  color: #fff;
  font-size: 24px;
  margin: 0;
}

nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

nav li {
  display: inline-block;
  margin-right: 10px;
}

nav a {
  color: #fff;
  text-decoration: none;
  padding: 5px 10px;
  font-size: 16px;
  font-weight: bold;
}

nav a:hover {
  text-decoration: underline;
}

.nav-link {
  transition: color 0.3s ease;
}

.nav-link:hover {
  color: #ffcc00;
}

.content {
  background: rgba(0, 0, 0, 0.5);
  padding: 20px;
  margin: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>