<template>
  <div class="upload-container">
    <h2>上傳資料</h2>
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="species">樹種：</label>
        <input id="species" v-model="form.species" type="text" placeholder="如：榕樹" />
      </div>
      <div class="form-group">
        <label for="age">推估樹齡（年）：</label>
        <input id="age" v-model.number="form.age" type="number" min="0" placeholder="如：10" />
      </div>
      <div class="form-group required">
        <label for="location">地點（必填）：</label>
        <input id="location" v-model="form.location" type="text" required placeholder="如：台北市信義區松智路1號" />
      </div>
      <div class="form-group required">
        <label for="district">行政區（必填，包含縣市）：</label>
        <input id="district" v-model="form.district" type="text" required placeholder="如：台北市信義區" />
      </div>
      <div class="form-group required">
        <label for="longitude">經度 X：</label>
        <input id="longitude" v-model="form.longitude" type="number" step="any" required placeholder="自動偵測或手動填寫" />
      </div>
      <div class="form-group required">
        <label for="latitude">緯度 Y：</label>
        <input id="latitude" v-model="form.latitude" type="number" step="any" required placeholder="自動偵測或手動填寫" />
        <button type="button" @click="detectLocation" style="margin-top:0.5rem;">偵測目前位置</button>
      </div>
      <div class="form-group">
        <label for="photo">照片：</label>
        <input id="photo" type="file" accept="image/*" @change="handlePhotoUpload" />
      </div>
      <div class="form-group">
        <label for="desc">狀況描述：</label>
        <textarea id="desc" v-model="form.description" rows="3" placeholder="請描述樹木狀況"></textarea>
      </div>
      <button type="submit">儲存到 localStorage</button>
    </form>
    <div v-if="savedData" class="saved-info">
      <h3>已儲存資料：</h3>
      <pre>{{ savedData }}</pre>
      <div v-if="savedData.photo">
        <img :src="savedData.photo" alt="上傳照片" style="max-width: 100%; margin-top: 1rem;" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface UploadForm {
  species: string
  age: number | null
  location: string
  district: string
  photo: string | null
  description: string
  longitude: number | null
  latitude: number | null
}

const STORAGE_KEY = 'user_upload_data'
const form = ref<UploadForm>({
  species: '',
  age: null,
  location: '',
  district: '',
  photo: null,
  description: '',
  longitude: null,
  latitude: null
})
const savedData = ref<any>(null)

onMounted(() => {
  const data = localStorage.getItem(STORAGE_KEY)
  if (data) {
    try {
      savedData.value = JSON.parse(data)
    } catch {
      savedData.value = data
    }
  }
})

function handlePhotoUpload(e: Event) {
  const file = (e.target as HTMLInputElement).files?.[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = () => {
      form.value.photo = reader.result as string
    }
    reader.readAsDataURL(file)
  }
}

function detectLocation() {
  if (!navigator.geolocation) {
    alert('瀏覽器不支援定位功能')
    return
  }
  navigator.geolocation.getCurrentPosition(
    (pos) => {
      form.value.longitude = pos.coords.longitude
      form.value.latitude = pos.coords.latitude
    },
    (err) => {
      alert('定位失敗: ' + err.message)
    }
  )
}

function handleSubmit() {
  if (!form.value.location || !form.value.district || form.value.longitude === null || form.value.latitude === null) {
    alert('請填寫必填欄位：地點、行政區、經度、緯度')
    return
  }
  localStorage.setItem(STORAGE_KEY, JSON.stringify(form.value))
  savedData.value = { ...form.value }
  // 清空表單
  form.value = {
    species: '',
    age: null,
    location: '',
    district: '',
    photo: null,
    description: '',
    longitude: null,
    latitude: null
  }
}
</script>

<style scoped>
.upload-container {
  max-width: 400px;
  margin: 2rem auto;
  padding: 2rem;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
}
.form-group {
  margin-bottom: 1rem;
  display: flex;
  flex-direction: column;
}
.form-group.required label:after {
  content: '*';
  color: #e74c3c;
  margin-left: 0.25em;
}
input[type="text"], input[type="number"], textarea {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}
input[type="file"] {
  margin-top: 0.5rem;
}
button {
  background: #667eea;
  color: #fff;
  border: none;
  padding: 0.5rem 1.5rem;
  border-radius: 4px;
  cursor: pointer;
}
.saved-info {
  margin-top: 1.5rem;
  background: #f8f9fa;
  padding: 1rem;
  border-radius: 6px;
  color: #333;
}
</style>
