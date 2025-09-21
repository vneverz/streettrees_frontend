<script setup lang="ts">
// 說明頁組件
</script>

<template>
  <div class="about">
    <div class="container">
      <div class="header">
        <h1>關於街樹地圖</h1>
        <p>讓我們一起了解這個專案的背景和目標</p>
      </div>

      <div class="content">
        <section class="section">
          <h2>專案背景</h2>
          <p>
            街樹地圖是一個致力於記錄和展示城市街樹資訊的平台。我們相信每一棵街樹都是城市生態系統中不可或缺的一部分，
            它們不僅美化環境，更為我們提供清新的空氣和舒適的陰涼。
          </p>
          <p>
            透過這個平台，我們希望能夠提高公眾對城市綠化的認識，並促進對街樹保護的重視。
          </p>
        </section>

        <section class="section">
          <h2>主要功能</h2>
          <div class="feature-list">
            <div class="feature-item">
              <div class="feature-icon">🗺️</div>
              <div class="feature-content">
                <h3>互動式地圖</h3>
                <p>透過直觀的地圖介面，輕鬆瀏覽和搜尋街樹位置</p>
              </div>
            </div>
            <div class="feature-item">
              <div class="feature-icon">📋</div>
              <div class="feature-content">
                <h3>詳細資訊</h3>
                <p>查看每棵樹的品種、年齡、健康狀況等詳細資料</p>
              </div>
            </div>
            <div class="feature-item">
              <div class="feature-icon">📊</div>
              <div class="feature-content">
                <h3>統計分析</h3>
                <p>了解不同區域的綠化覆蓋率和樹種分布情況</p>
              </div>
            </div>
            <div class="feature-item">
              <div class="feature-icon">👥</div>
              <div class="feature-content">
                <h3>社群參與</h3>
                <p>鼓勵民眾參與街樹的記錄和保護工作</p>
              </div>
            </div>
          </div>
        </section>

        <section class="section">
          <h2>技術特色</h2>
          <div class="tech-grid">
            <div class="tech-card">
              <h3>Vue 3</h3>
              <p>使用最新的Vue 3框架，提供流暢的使用者體驗</p>
            </div>
            <div class="tech-card">
              <h3>TypeScript</h3>
              <p>強型別支援，確保程式碼品質和開發效率</p>
            </div>
            <div class="tech-card">
              <h3>響應式設計</h3>
              <p>適配各種裝置尺寸，提供一致的使用體驗</p>
            </div>
            <div class="tech-card">
              <h3>現代化UI</h3>
              <p>簡潔美觀的介面設計，提升使用者互動體驗</p>
            </div>
          </div>
        </section>

        <section class="section">
          <h2>聯絡我們</h2>
          <p>
            如果您對街樹地圖有任何建議或問題，歡迎與我們聯絡。
            我們期待聽到您的聲音，一起為城市綠化貢獻心力。
          </p>
          <div class="contact-info">
            <div class="contact-item">
              <strong>電子郵件：</strong>
              <span>contact@streettrees.tw</span>
            </div>
            <div class="contact-item">
              <strong>專案網址：</strong>
              <span>https://github.com/streettrees</span>
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<style scoped>
.about {
  min-height: calc(100vh - 80px);
  background-color: #f8f9fa;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

.header {
  text-align: center;
  margin-bottom: 3rem;
}

.header h1 {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 1rem;
}

.header p {
  font-size: 1.2rem;
  color: #7f8c8d;
}

.content {
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.section {
  background: white;
  padding: 2rem;
  border-radius: 12px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.section h2 {
  color: #2c3e50;
  margin-bottom: 1.5rem;
  font-size: 1.8rem;
}

.section p {
  color: #555;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.feature-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
}

.feature-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  padding: 1rem;
  border-radius: 8px;
  background-color: #f8f9fa;
}

.feature-icon {
  font-size: 2rem;
  flex-shrink: 0;
}

.feature-content h3 {
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

.feature-content p {
  color: #7f8c8d;
  margin: 0;
}

.tech-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}

.tech-card {
  padding: 1.5rem;
  border-radius: 8px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
}

.tech-card h3 {
  margin-bottom: 1rem;
  font-size: 1.3rem;
}

.tech-card p {
  margin: 0;
  opacity: 0.9;
}

.contact-info {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.contact-item {
  display: flex;
  gap: 0.5rem;
}

.contact-item strong {
  color: #2c3e50;
  min-width: 100px;
}

.contact-item span {
  color: #3498db;
}

@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }

  .header h1 {
    font-size: 2rem;
  }

  .section {
    padding: 1.5rem;
  }

  .feature-item {
    flex-direction: column;
    text-align: center;
  }

  .contact-item {
    flex-direction: column;
  }

  .contact-item strong {
    min-width: auto;
  }
}
</style>
