<script setup lang="ts">
import { ref, onMounted } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

// 組件名稱
defineOptions({
  name: 'AppMapView'
})

// 修復Leaflet預設圖標問題
delete (L.Icon.Default.prototype as any)._getIconUrl
L.Icon.Default.mergeOptions({
  iconRetinaUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/images/marker-icon-2x.png',
  iconUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/images/marker-icon.png',
  shadowUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.7.1/images/marker-shadow.png',
})

const mapContainer = ref<HTMLElement>()
let map: L.Map | null = null

// 樣稿街樹數據
const sampleTrees = [
  {
    id: 1,
    name: '榕樹',
    species: 'Ficus microcarpa',
    lat: 25.0330,
    lng: 121.5654,
    age: 15,
    health: '良好',
    description: '位於台北101附近的古老榕樹，樹冠茂密，提供良好的遮蔭。',
    plantedDate: '2009-03-15'
  },
  {
    id: 2,
    name: '樟樹',
    species: 'Cinnamomum camphora',
    lat: 25.0340,
    lng: 121.5664,
    age: 8,
    health: '良好',
    description: '香氣濃郁的樟樹，具有驅蟲效果，是城市綠化的優良樹種。',
    plantedDate: '2016-05-20'
  },
  {
    id: 3,
    name: '楓香',
    species: 'Liquidambar formosana',
    lat: 25.0320,
    lng: 121.5644,
    age: 12,
    health: '良好',
    description: '秋季葉片會變紅，是觀賞價值很高的行道樹。',
    plantedDate: '2012-11-10'
  },
  {
    id: 4,
    name: '台灣欒樹',
    species: 'Koelreuteria henryi',
    lat: 25.0350,
    lng: 121.5674,
    age: 6,
    health: '良好',
    description: '台灣原生種，夏季開黃花，秋季結紅果，四季變化豐富。',
    plantedDate: '2018-08-25'
  },
  {
    id: 5,
    name: '鳳凰木',
    species: 'Delonix regia',
    lat: 25.0310,
    lng: 121.5634,
    age: 10,
    health: '良好',
    description: '夏季開紅花，是熱帶地區常見的觀賞樹種。',
    plantedDate: '2014-06-12'
  },
  {
    id: 6,
    name: '木棉',
    species: 'Bombax ceiba',
    lat: 25.0360,
    lng: 121.5684,
    age: 7,
    health: '良好',
    description: '春季開紅花，花朵大而美麗，是優良的觀賞樹種。',
    plantedDate: '2017-04-08'
  }
]

onMounted(() => {
  if (mapContainer.value) {
    // 初始化地圖，以台北101為中心
    map = L.map(mapContainer.value).setView([25.0330, 121.5654], 15)

    // 添加OpenStreetMap圖層
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors'
    }).addTo(map)

    // 添加街樹標記
    sampleTrees.forEach(tree => {
      // 創建自定義圖標
      const treeIcon = L.divIcon({
        className: 'custom-tree-icon',
        html: `<div class="tree-marker">
                 <div class="tree-icon">🌳</div>
                 <div class="tree-name">${tree.name}</div>
               </div>`,
        iconSize: [40, 50],
        iconAnchor: [20, 50]
      })

      // 創建標記
      const marker = L.marker([tree.lat, tree.lng], { icon: treeIcon })
        .addTo(map!)

      // 添加彈出視窗
      marker.bindPopup(`
        <div class="tree-popup">
          <h3>${tree.name}</h3>
          <p><strong>學名：</strong>${tree.species}</p>
          <p><strong>樹齡：</strong>${tree.age}年</p>
          <p><strong>健康狀況：</strong>${tree.health}</p>
          <p><strong>種植日期：</strong>${tree.plantedDate}</p>
          <p><strong>描述：</strong>${tree.description}</p>
        </div>
      `)
    })
  }
})
</script>

<template>
  <div class="map-container">
    <div class="map-header">
      <h2>街樹地圖</h2>
      <p>探索城市中的綠色寶藏，點擊標記查看詳細資訊</p>
    </div>
    <div ref="mapContainer" class="map"></div>
    <div class="map-legend">
      <div class="legend-item">
        <span class="legend-icon">🌳</span>
        <span>街樹位置</span>
      </div>
      <div class="legend-item">
        <span class="legend-color good"></span>
        <span>健康良好</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.map-container {
  width: 100%;
  height: 600px;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background: white;
}

.map-header {
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  text-align: center;
}

.map-header h2 {
  margin: 0 0 0.5rem 0;
  font-size: 1.8rem;
}

.map-header p {
  margin: 0;
  opacity: 0.9;
}

.map {
  height: 450px;
  width: 100%;
}

.map-legend {
  display: flex;
  justify-content: center;
  gap: 2rem;
  padding: 1rem;
  background: #f8f9fa;
  border-top: 1px solid #e9ecef;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
  color: #555;
}

.legend-icon {
  font-size: 1.2rem;
}

.legend-color {
  width: 16px;
  height: 16px;
  border-radius: 50%;
}

.legend-color.good {
  background-color: #28a745;
}

/* 自定義標記樣式 */
:deep(.custom-tree-icon) {
  background: transparent;
  border: none;
}

:deep(.tree-marker) {
  text-align: center;
  cursor: pointer;
}

:deep(.tree-icon) {
  font-size: 24px;
  margin-bottom: 2px;
}

:deep(.tree-name) {
  font-size: 10px;
  font-weight: bold;
  color: #2c3e50;
  background: rgba(255, 255, 255, 0.9);
  padding: 2px 4px;
  border-radius: 4px;
  white-space: nowrap;
}

/* 彈出視窗樣式 */
:deep(.leaflet-popup-content) {
  margin: 0;
  padding: 0;
}

:deep(.tree-popup) {
  padding: 1rem;
  min-width: 250px;
}

:deep(.tree-popup h3) {
  margin: 0 0 1rem 0;
  color: #2c3e50;
  font-size: 1.2rem;
}

:deep(.tree-popup p) {
  margin: 0.5rem 0;
  color: #555;
  line-height: 1.4;
}

:deep(.tree-popup strong) {
  color: #2c3e50;
}

@media (max-width: 768px) {
  .map-container {
    height: 500px;
  }

  .map {
    height: 350px;
  }

  .map-header h2 {
    font-size: 1.5rem;
  }

  .map-legend {
    flex-direction: column;
    gap: 0.5rem;
  }
}
</style>
