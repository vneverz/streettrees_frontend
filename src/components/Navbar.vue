<script setup lang="ts">
import { RouterLink } from 'vue-router'

// 組件名稱
defineOptions({
  name: 'AppNavbar'
})
</script>

<template>
  <nav class="navbar">
    <div class="navbar-brand">
      <RouterLink to="/" class="brand-link">路樹地圖</RouterLink>
    </div>
    <div class="navbar-menu">
      <RouterLink to="/" class="nav-link">首頁</RouterLink>
      <RouterLink to="/about" class="nav-link">說明</RouterLink>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #2c3e50;
  color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.navbar-brand {
  font-size: 1.5rem;
  font-weight: bold;
}

.brand-link {
  color: white;
  text-decoration: none;
  transition: color 0.3s ease;
}

.brand-link:hover {
  color: #3498db;
}

.navbar-menu {
  display: flex;
  gap: 2rem;
}

.nav-link {
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

.nav-link:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.nav-link.router-link-active {
  background-color: #3498db;
}
</style>
