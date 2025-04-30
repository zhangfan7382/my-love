<template>
    <div class="message-card">
      <h1 class="title">💝 土味情话 💝</h1>
      <div class="message-box" @touchstart="onTouchStart" @touchend="onTouchEnd">
        <p class="message-text">{{ currentMessage }}</p>
      </div>
      <button 
        class="get-message-btn" 
        @click="getLoveMessage" 
        :disabled="isLoading"
      >
        {{ buttonText }}
      </button>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  
  const currentMessage = ref('正在加载情话...')
  const isLoading = ref(false)
  const buttonText = ref('点击获取帆帆想对你说的话')
  
  const API_URL = 'https://api.uomg.com/api/rand.qinghua?format=json'
  
  const backupMessages = [
    "你是我最想留住的幸运",
    "遇见你是我最美丽的意外",
    "我的心里只装得下你一个人",
    "你是我最特别的人",
    "我愿意为你付出所有",
    "你就是我的世界",
    "每一天，我都在想你",
    "你的笑容是我最大的幸福",
    "有你的每一天都是晴天",
    "你是我生命中最亮的星"
  ]
  
  const getLoveMessage = async () => {
    if (isLoading.value) return
    
    isLoading.value = true
    buttonText.value = '获取中...'
    currentMessage.value = '正在加载情话...'
  
    try {
      const response = await fetch(API_URL)
      if (!response.ok) {
        throw new Error('网络请求失败')
      }
      const data = await response.json()
      if (data.code === 1 && data.content) {
        currentMessage.value = data.content
      } else {
        throw new Error('API返回格式错误')
      }
    } catch (error) {
      console.error('获取情话失败:', error)
      // 使用本地备用情话
      const randomIndex = Math.floor(Math.random() * backupMessages.length)
      currentMessage.value = backupMessages[randomIndex]
    } finally {
      isLoading.value = false
      buttonText.value = '点击获取帆帆想对你说的话'
    }
  }
  
  const onTouchStart = (e) => {
    e.target.style.transform = 'scale(0.98)'
  }
  
  const onTouchEnd = (e) => {
    e.target.style.transform = 'scale(1)'
  }
  
  onMounted(() => {
    getLoveMessage()
  })
  </script>
  
  <style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .message-card {
    background: rgba(255, 255, 255, 0.95);
    border-radius: 20px;
    padding: 40px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    text-align: center;
    max-width: 600px;
    width: 90%;
  }
  
  .title {
    color: #ff6b6b;
    font-size: 2.5em;
    margin-bottom: 30px;
    font-weight: bold;
  }
  
  .message-box {
    background: #fff5f5;
    border-radius: 15px;
    padding: 20px;
    margin: 20px 0;
    min-height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.2s ease;
  }
  
  .message-text {
    font-size: 1.2em;
    color: #ff4757;
    line-height: 1.6;
    margin: 0;
  }
  
  .get-message-btn {
    background: #ff6b6b;
    color: white;
    border: none;
    padding: 15px 30px;
    border-radius: 25px;
    font-size: 1.1em;
    cursor: pointer;
    transition: all 0.3s ease;
    margin-top: 20px;
  }
  
  .get-message-btn:hover {
    background: #ff4757;
    transform: translateY(-2px);
  }
  
  .get-message-btn:disabled {
    background: #ffb8b8;
    cursor: not-allowed;
    transform: none;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  .message-box {
    animation: fadeIn 0.5s ease-out;
  }
  </style> 