<template>
  <div class="upload-container">
    <h2>上傳資料</h2>
    <!-- 問使用者是否允許自動偵測位置（只在尚未選擇時顯示） -->
    <div v-if="showPermissionPrompt" class="permission-prompt">
      <p>是否允許此頁面自動偵測您目前的位置，以便填入經緯度？</p>
      <div class="prompt-actions">
        <button type="button" @click="handleAllow">允許</button>
        <button type="button" class="deny" @click="handleDeny">不同意</button>
      </div>
    </div>

    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">樹名（顯示名稱）：</label>
        <input id="name" v-model="form.name" type="text" placeholder="如：樹王" />
      </div>

      <div class="form-group">
        <label for="species">樹種：</label>
        <input id="species" v-model="form.species" type="text" placeholder="如：Ficus microcarpa" />
      </div>

      <div class="form-group">
        <label for="age">推估樹齡（年）：</label>
        <input id="age" v-model.number="form.age" type="number" min="0" placeholder="如：20" />
      </div>

      <div class="form-group required">
        <label for="location">地點（必填）：</label>
        <input id="location" v-model="form.location" type="text" required placeholder="如：Main Street No.5" />
      </div>

      <div class="form-group required">
        <label for="district">行政區（必填，包含縣市）：</label>
        <input id="district" v-model="form.district" type="text" required placeholder="如：Taipei" />
      </div>

      <div class="form-group required">
        <label for="lng">經度 X（lng）：</label>
        <input id="lng" v-model.number="form.lng" type="number" step="any" required placeholder="自動偵測或手動填寫" />
      </div>

      <div class="form-group required">
        <label for="lat">緯度 Y（lat）：</label>
        <input id="lat" v-model.number="form.lat" type="number" step="any" required placeholder="自動偵測或手動填寫" />
        <button type="button" class="detect-btn" @click="detectLocation" style="margin-top:0.5rem;">偵測目前位置</button>
      </div>

      <div class="form-group">
        <label for="photo">照片：</label>
        <input id="photo" type="file" accept="image/*" @change="handlePhotoUpload" />
      </div>

      <div class="form-group">
        <label for="desc">狀況描述：</label>
        <textarea id="desc" v-model="form.description" rows="3" placeholder="請描述樹木狀況"></textarea>
      </div>

      <button type="submit">上傳到後端</button>
    </form>

    <div v-if="savedData" class="saved-info">
      <h3>已儲存資料（回應）：</h3>
      <pre>{{ formatSavedData }}</pre>
      <div v-if="savedPhotoSrc">
        <img :src="savedPhotoSrc" alt="上傳照片" style="max-width: 100%; margin-top: 1rem;" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'

interface UploadForm {
  name: string
  species: string
  age: number | null
  location: string
  district: string
  photo: string | null
  description: string
  lat: number | null
  lng: number | null
}

const STORAGE_KEY = 'user_upload_data'
const form = ref<UploadForm>({
  name: '',
  species: '',
  age: null,
  location: '',
  district: '',
  photo: null,
  description: '',
  lat: null,
  lng: null
})
const savedData = ref<Record<string, unknown> | null>(null)

// 權限與偵測狀態
const showPermissionPrompt = ref(false)
const isDetecting = ref(false)
const permissionKey = 'geo_permission_session'
// 持久化的 permission key（會存在 localStorage）
const persistentPermissionKey = 'geo_permission'
// promptShownKey：表示是否已經顯示過提示（只要顯示過一次就不再顯示）
const promptShownKey = 'geo_prompt_shown'
// 先檢查 sessionStorage，沒有則檢查 localStorage（持久化）
const permission = ref<string | null>(sessionStorage.getItem(permissionKey) ?? localStorage.getItem(persistentPermissionKey))

onMounted(() => {
  const data = localStorage.getItem(STORAGE_KEY)
  if (data) {
    try {
      savedData.value = JSON.parse(data)
    } catch {
      savedData.value = data as unknown as Record<string, unknown>
    }
  }

  // 根據 session 或本地持久化的選擇決定是否自動偵測
  if (permission.value === 'granted') {
    showPermissionPrompt.value = false
    detectLocation()
  } else if (permission.value === 'denied') {
    showPermissionPrompt.value = false
  } else {
    // 尚未有明確選擇：只在第一次載入（未顯示過提示）時顯示提示
    const shown = localStorage.getItem(promptShownKey)
    if (!shown) {
      showPermissionPrompt.value = true
      localStorage.setItem(promptShownKey, 'true')
    } else {
      showPermissionPrompt.value = false
    }
  }
})

const selectedFile = ref<File | null>(null)

function handlePhotoUpload(e: Event) {
  const file = (e.target as HTMLInputElement).files?.[0]
  if (file) {
    selectedFile.value = file
    const reader = new FileReader()
    reader.onload = () => {
      // keep a preview/data URL so we can show it immediately as fallback
      form.value.photo = reader.result as string
    }
    reader.readAsDataURL(file)
  }
}

const uploadInProgress = ref(false)

async function uploadToCloudflare(file: File): Promise<string> {
  const accountId = import.meta.env.VITE_CF_ACCOUNT_ID
  const token = import.meta.env.VITE_CF_API_TOKEN
  const base = import.meta.env.VITE_CF_IMAGES_BASE_URL // optional base to build imagedelivery URL

  if (!accountId || !token) throw new Error('Cloudflare Images credentials not configured')

  const fd = new FormData()
  fd.append('file', file)

  const res = await fetch(`https://api.cloudflare.com/client/v4/accounts/${accountId}/images/v1`, {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${token}`
    },
    body: fd
  })
  const json = await res.json()
  if (!json?.success) throw new Error(json?.errors?.[0]?.message || 'Cloudflare upload failed')
  const result = json.result
  // Prefer returned variant URL if available
  if (result?.variants && result.variants.length > 0) return result.variants[0]
  // If user provided a base URL (e.g. https://imagedelivery.net/<account_hash>), build a URL
  if (base && result?.id) return `${base}/${result.id}/public`
  // If nothing else, return the id (caller can decide)
  if (result?.id) return String(result.id)
  throw new Error('No usable URL returned from Cloudflare')
}

function detectLocation() {
  if (!navigator.geolocation) {
    alert('瀏覽器不支援定位功能')
    return
  }
  isDetecting.value = true
  navigator.geolocation.getCurrentPosition(
    (pos) => {
      form.value.lng = Number(pos.coords.longitude)
      form.value.lat = Number(pos.coords.latitude)
      isDetecting.value = false
    },
    (err) => {
      isDetecting.value = false
      alert('定位失敗: ' + err.message)
    },
    { enableHighAccuracy: true, timeout: 10000 }
  )
}

function handleAllow() {
  // 記錄在 sessionStorage（本次 session）與 localStorage（持久化）
  sessionStorage.setItem(permissionKey, 'granted')
  localStorage.setItem(persistentPermissionKey, 'granted')
  permission.value = 'granted'
  showPermissionPrompt.value = false
  detectLocation()
}

function handleDeny() {
  sessionStorage.setItem(permissionKey, 'denied')
  permission.value = 'denied'
  showPermissionPrompt.value = false
}

const formatSavedData = computed(() => {
  return savedData.value ? JSON.stringify(savedData.value, null, 2) : ''
})

const savedPhotoSrc = computed<string | null>(() => {
  if (!savedData.value) return null
  const p = (savedData.value as Record<string, unknown>).photo
  return typeof p === 'string' ? p : null
})

async function handleSubmit() {
  if (!form.value.location || !form.value.district || form.value.lng === null || form.value.lat === null) {
    alert('請填寫必填欄位：地點、行政區、經度、緯度')
    return
  }
  // 建立 payload 符合後端格式
  const payload = {
    name: form.value.name,
    species: form.value.species,
    age: form.value.age,
    location: form.value.location,
    district: form.value.district,
    photo: form.value.photo,
    description: form.value.description,
    lat: form.value.lat,
    lng: form.value.lng,
    created_at: new Date().toISOString().replace('T', ' ').slice(0, 19)
  }

  // 先嘗試 POST 到後端 API，若失敗則回退到 localStorage
  const TREES_API = 'https://streettrees-backend.vneverz.workers.dev/trees'
  // If there's a selected File and Cloudflare credentials exist, try uploading the image first
  if (selectedFile.value) {
    const hasCF = import.meta.env.VITE_CF_ACCOUNT_ID && import.meta.env.VITE_CF_API_TOKEN
    if (hasCF) {
      uploadInProgress.value = true
      try {
        const url = await uploadToCloudflare(selectedFile.value)
        payload.photo = url
      } catch (err) {
        // fallback: keep data URL preview already in form.value.photo
        console.warn('Cloudflare upload failed, using data URL fallback:', err)
        payload.photo = form.value.photo
      } finally {
        uploadInProgress.value = false
      }
    } else {
      // No external uploader configured; use data URL preview
      payload.photo = form.value.photo
    }
  }

  try {
    const res = await fetch(TREES_API, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    })
    if (res.ok) {
      const data = await res.json().catch(() => payload)
      savedData.value = data as Record<string, unknown>
      // 可選：把最新資料存到 localStorage 作為備份
      localStorage.setItem(STORAGE_KEY, JSON.stringify(data))
    } else {
      // fallback
      localStorage.setItem(STORAGE_KEY, JSON.stringify(payload))
      savedData.value = payload as Record<string, unknown>
    }
  } catch {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(payload))
    savedData.value = payload as Record<string, unknown>
  }

  // 清空表單
  form.value = {
    name: '',
    species: '',
    age: null,
    location: '',
    district: '',
    photo: null,
    description: '',
    lat: null,
    lng: null
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
