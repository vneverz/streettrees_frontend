<script setup lang="ts">
import MapView from '../components/MapView.vue'
</script>

<template>
  <div class="home">
    <div class="hero">
      <h1>歡迎來到街樹地圖</h1>
      <p>探索城市中的綠色寶藏，發現身邊的美麗街樹</p>
      <div class="hero-actions">
        <button class="btn-primary">開始探索</button>
        <button class="btn-secondary">了解更多</button>
      </div>
    </div>

    <div class="features">
      <div class="feature-card">
        <h3>🌳 街樹資訊</h3>
        <p>查看詳細的樹種資訊、生長狀況和維護記錄</p>
      </div>
      <div class="feature-card">
        <h3>📍 地圖定位</h3>
        <p>透過互動式地圖快速找到附近的街樹位置</p>
      </div>
      <div class="feature-card">
        <h3>📊 數據統計</h3>
        <p>了解城市綠化覆蓋率和樹種分布統計</p>
      </div>
    </div>

    <!-- 街樹地圖區域 -->
    <div class="map-section">
      <MapView />
    </div>
  </div>
</template>

<style scoped>
.home {
  min-height: calc(100vh - 80px);
}

.hero {
  text-align: center;
  padding: 4rem 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.hero h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
  font-weight: 700;
}

.hero p {
  font-size: 1.2rem;
  margin-bottom: 2rem;
  opacity: 0.9;
}

.hero-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
}

.btn-primary,
.btn-secondary {
  padding: 0.75rem 2rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary {
  background-color: #3498db;
  color: white;
}

.btn-primary:hover {
  background-color: #2980b9;
  transform: translateY(-2px);
}

.btn-secondary {
  background-color: transparent;
  color: white;
  border: 2px solid white;
}

.btn-secondary:hover {
  background-color: white;
  color: #667eea;
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  padding: 4rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.feature-card {
  text-align: center;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background: white;
  transition: transform 0.3s ease;
}

.feature-card:hover {
  transform: translateY(-5px);
}

.feature-card h3 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #2c3e50;
}

.feature-card p {
  color: #7f8c8d;
  line-height: 1.6;
}

.map-section {
  padding: 2rem;
  background-color: #f8f9fa;
}

@media (max-width: 768px) {
  .hero h1 {
    font-size: 2rem;
  }

  .hero p {
    font-size: 1rem;
  }

  .hero-actions {
    flex-direction: column;
    align-items: center;
  }

  .btn-primary,
  .btn-secondary {
    width: 200px;
  }

  .map-section {
    padding: 1rem;
  }
}
</style>
