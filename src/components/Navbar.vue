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
      <RouterLink to="/" class="nav-link">
        <svg class="nav-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z" />
          <polyline points="9,22 9,12 15,12 15,22" />
        </svg>
        <span class="nav-text">首頁</span>
      </RouterLink>
      <RouterLink to="/about" class="nav-link">
        <svg class="nav-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="10" />
          <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3" />
          <line x1="12" y1="17" x2="12.01" y2="17" />
        </svg>
        <span class="nav-text">說明</span>
      </RouterLink>
      <RouterLink to="/upload" class="nav-link">
        <svg class="nav-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" />
          <polyline points="7,14 12,9 17,14" />
          <line x1="12" y1="9" x2="12" y2="21" />
        </svg>
        <span class="nav-text">上傳</span>
      </RouterLink>
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
  gap: 1rem;
}

.nav-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: white;
  text-decoration: none;
  padding: 0.5rem 0.75rem;
  border-radius: 8px;
  transition: all 0.3s ease;
  min-width: 60px;
}

.nav-link:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

.nav-link.router-link-active {
  background-color: #3498db;
}

.nav-icon {
  width: 20px;
  height: 20px;
  margin-bottom: 2px;
}

.nav-text {
  font-size: 0.75rem;
  font-weight: 500;
  white-space: nowrap;
}

/* 手機版RWD */
@media (max-width: 768px) {
  .navbar {
    padding: 0.75rem 1rem;
    justify-content: center;
  }

  .navbar-brand {
    display: none;
  }

  .navbar-menu {
    gap: 0.5rem;
    width: 100%;
    justify-content: space-around;
  }

  .nav-link {
    padding: 0.5rem 0.25rem;
    min-width: 50px;
  }

  .nav-icon {
    width: 18px;
    height: 18px;
  }

  .nav-text {
    font-size: 0.7rem;
  }
}

/* 超小螢幕 */
@media (max-width: 480px) {
  .navbar {
    padding: 0.5rem 0.5rem;
  }

  .navbar-menu {
    gap: 0.25rem;
  }

  .nav-link {
    padding: 0.4rem 0.2rem;
    min-width: 45px;
  }

  .nav-icon {
    width: 16px;
    height: 16px;
  }

  .nav-text {
    font-size: 0.65rem;
  }
}
</style>
