<script setup>
import { ref, onMounted } from 'vue'
import QingmingFlower from '../components/QingmingFlower.vue'

// 是否正在播放音乐
const isPlaying = ref(false)
const audio = ref(null)
const flowerCount = ref(0)
const showMemorial = ref(false)
const isLoading = ref(true)
const showEntranceCandle = ref(false)
const candleLit = ref(false)

// 创建音频对象
function createAudio() {
  // 创建音频对象，使用在线资源或者本地资源
  audio.value = new Audio()
  
  // 设置音频属性
  audio.value.loop = true
  
  // 尝试使用在线音频资源（可以替换成其他清明节相关的音乐）
  audio.value.src = 'https://file-examples.com/storage/fe31c6155beed903ab08559/2017/11/file_example_MP3_700KB.mp3'
  
  // 预加载但不自动播放
  audio.value.preload = 'auto'
  audio.value.load()
}

// 播放/暂停背景音乐
function toggleMusic() {
  if (!audio.value) return
  
  if (isPlaying.value) {
    audio.value.pause()
  } else {
    // 尝试播放音频
    const playPromise = audio.value.play()
    
    // 处理自动播放策略问题
    if (playPromise !== undefined) {
      playPromise.catch(error => {
        console.log('音频播放失败:', error)
      })
    }
  }
  isPlaying.value = !isPlaying.value
}

// 献花
function offerFlower() {
  flowerCount.value++
  
  // 创建动画效果
  const container = document.querySelector('.qingming-container')
  const flower = document.createElement('div')
  flower.className = 'flying-flower'
  flower.style.left = Math.random() * 80 + 10 + '%'
  container.appendChild(flower)
  
  // 移除元素
  setTimeout(() => {
    container.removeChild(flower)
  }, 3000)
}

// 显示祭奠弹窗
function showMemorialDialog() {
  showMemorial.value = true
}

// 关闭祭奠弹窗
function closeMemorial() {
  showMemorial.value = false
}

// 点亮蜡烛进入主页面
function lightCandle() {
  if (candleLit.value) return
  
  candleLit.value = true
  
  // 添加点亮效果
  const flame = document.querySelector('.entrance-flame')
  if (flame) {
    flame.classList.add('lit')
  }
  
  // 延迟后显示主页面
  setTimeout(() => {
    showEntranceCandle.value = false
  }, 2000)
}

// 页面加载时创建音频对象
onMounted(() => {
  createAudio()
  
  // 模拟页面加载完成，2秒后显示入口蜡烛
  setTimeout(() => {
    isLoading.value = false
    showEntranceCandle.value = true
  }, 500)
})
</script>

<template>
  <!-- 加载动画 -->
  <div class="loading-container" v-if="isLoading">
    <div class="loading-candle">
      <div class="loading-flame"></div>
    </div>
    <p class="loading-text">正在建立联系...</p>
  </div>
  
  <!-- 入口蜡烛 -->
  <div class="entrance-container" v-else-if="showEntranceCandle">
    <div class="entrance-candle" @click="lightCandle">
      <div class="entrance-flame" :class="{ 'lit': candleLit }"></div>
    </div>
    <p class="entrance-text" v-if="!candleLit">点亮蜡烛，打破空间</p>
    <p class="entrance-text entrance-text-lit" v-else>思念能跨越一切</p>
  </div>
  
  <div class="qingming-container" v-else>
    <div class="music-control" @click="toggleMusic">
      <i :class="isPlaying ? 'icon-pause' : 'icon-play'"></i>
    </div>
    
    <!-- 标题区域 -->
    <div class="header">
      <h1>欢迎来看我</h1>
      <p class="subtitle">xxxx, xxxxxx</p>
    </div>
    
    <!-- 主要内容区 -->
    <div class="content">
      <div class="poem">
        <p>游人记得承平事，</p>
        <p>暗喜风光似昔年。</p>
        <small>— 韦庄《长安清明》</small>
      </div>
      
      <div class="memorial">
        <div class="candle"></div>
        <div class="flower-wrapper">
          <QingmingFlower :size="60" color="#ffffff" />
        </div>
        <div class="flower-count" v-if="flowerCount > 0">
          献花 × {{ flowerCount }}
        </div>
        <div class="tribute-actions">
          <button class="tribute-btn" @click="offerFlower">献花</button>
          <button class="tribute-btn" @click="showMemorialDialog">寄语</button>
        </div>
      </div>
    </div>
    
    <!-- 底部信息 -->
    <div class="footer">
      <p>逝者已逝，生者如斯</p>
      <p class="copyright">© {{ new Date().getFullYear() }} 扫墓</p>
    </div>
    
    <!-- 祭奠弹窗 -->
    <div class="memorial-dialog" v-if="showMemorial">
      <div class="memorial-dialog-content">
        <div class="dialog-header">
          <h2>寄语追思</h2>
          <button class="close-btn" @click="closeMemorial">×</button>
        </div>
        <div class="dialog-body">
          <textarea placeholder="写下想对我说的话..." rows="5"></textarea>
          <div class="dialog-flowers">
            <QingmingFlower :size="30" color="#ffffff" />
            <QingmingFlower :size="30" color="#ffffff" />
            <QingmingFlower :size="30" color="#ffffff" />
          </div>
          <button class="submit-btn">寄语上传</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 加载动画样式 */
.loading-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom, #1a1a2e, #16213e);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.loading-candle {
  width: 40px;
  height: 120px;
  background: linear-gradient(to top, #f5f5f5, #e0e0e0);
  border-radius: 15px 15px 0 0;
  position: relative;
  margin-bottom: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.loading-flame {
  position: absolute;
  top: -35px;
  left: 15px;
  width: 10px;
  height: 35px;
  background: #ffc107;
  border-radius: 50% 50% 20% 20%;
  transform-origin: center bottom;
  box-shadow: 
    0 0 20px #ffc107, 
    0 0 40px #ff9800,
    0 0 60px #ff5722;
  animation: 
    loadingFlicker 0.8s ease-in-out infinite alternate,
    loadingPulse 1.5s ease-in-out infinite;
}

.loading-text {
  color: #fff;
  font-size: 1.2rem;
  margin-top: 2rem;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
  animation: textPulse 1.5s infinite alternate;
}

/* 入口蜡烛 */
.entrance-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom, #1a1a2e, #16213e);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  animation: fadeIn 0.5s ease;
}

.entrance-candle {
  width: 80px;
  height: 240px;
  background: linear-gradient(to top, #f5f5f5, #e0e0e0);
  border-radius: 20px 20px 0 0;
  position: relative;
  margin-bottom: 3rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  transition: transform 0.3s ease;
  animation: zoomIn 1s ease forwards;
}

.entrance-candle:hover {
  transform: scale(1.05);
}

.entrance-flame {
  position: absolute;
  top: -50px;
  left: 30px;
  width: 20px;
  height: 0;
  background: transparent;
  border-radius: 50% 50% 20% 20%;
  transform-origin: center bottom;
  transition: all 0.8s ease;
  opacity: 0;
}

.entrance-flame.lit {
  height: 70px;
  background: #ff9800;
  opacity: 1;
  box-shadow: 
    0 0 30px #ffc107, 
    0 0 60px #ff9800,
    0 0 90px #ff5722;
  animation: 
    entranceFlicker 1s ease-in-out infinite alternate,
    entranceGlow 2s ease-in-out forwards;
}

.entrance-text {
  color: #fff;
  font-size: 1.5rem;
  margin-top: 1rem;
  text-align: center;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
  opacity: 0.8;
  transition: all 0.5s ease;
}

.entrance-text-lit {
  color: #ffcc80;
  font-size: 1.8rem;
  opacity: 1;
  text-shadow: 
    0 0 10px rgba(255, 193, 7, 0.5),
    0 0 20px rgba(255, 152, 0, 0.3);
}

@keyframes zoomIn {
  from {
    transform: scale(0.5);
    opacity: 0.5;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes entranceFlicker {
  0%, 100% {
    transform: scaleY(1) rotate(2deg);
    opacity: 1;
  }
  25% {
    transform: scaleY(0.9) rotate(-2deg);
    opacity: 0.9;
  }
  50% {
    transform: scaleY(1.1) rotate(1deg);
    opacity: 1;
  }
  75% {
    transform: scaleY(0.95) rotate(-1deg);
    opacity: 0.95;
  }
}

@keyframes entranceGlow {
  from {
    box-shadow: 
      0 0 30px #ffc107, 
      0 0 60px #ff9800,
      0 0 90px #ff5722;
  }
  to {
    box-shadow: 
      0 0 50px #ffc107, 
      0 0 100px #ff9800,
      0 0 150px #ff5722;
  }
}

@keyframes loadingFlicker {
  0%, 100% {
    transform: scaleY(1) rotate(3deg);
    opacity: 1;
  }
  25% {
    transform: scaleY(0.8) rotate(-1deg);
    opacity: 0.8;
  }
  50% {
    transform: scaleY(1.2) rotate(1deg);
    opacity: 0.9;
  }
  75% {
    transform: scaleY(0.9) rotate(-3deg);
    opacity: 0.85;
  }
}

@keyframes loadingPulse {
  0%, 100% {
    box-shadow: 
      0 0 20px #ffc107, 
      0 0 40px #ff9800,
      0 0 60px #ff5722;
  }
  50% {
    box-shadow: 
      0 0 30px #ffc107, 
      0 0 60px #ff9800,
      0 0 90px #ff5722;
  }
}

@keyframes textPulse {
  from {
    opacity: 0.6;
  }
  to {
    opacity: 1;
  }
}

.qingming-container {
  min-height: 100vh;
  background: linear-gradient(to bottom, #1a1a2e, #16213e);
  color: #fff;
  text-align: center;
  display: flex;
  flex-direction: column;
  padding: 2rem 1rem;
  position: relative;
  overflow: hidden;
}

.qingming-container::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.4)), url('@/assets/willow.svg');
  background-size: cover;
  background-position: center bottom;
  opacity: 0.2;
  z-index: -1;
}

/* 雨滴动画 */
.qingming-container::after {
  content: "";
  position: absolute;
  top: -10%;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    rgba(255, 255, 255, 0.2) 100%
  );
  animation: rain 15s linear infinite;
  z-index: -1;
}

@keyframes rain {
  0% {
    transform: translateY(-10%);
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    transform: translateY(100%);
    opacity: 0;
  }
}

.header {
  margin-bottom: 4rem;
  animation: fadeIn 2s ease;
}

h1 {
  font-size: 3rem;
  margin-bottom: 0.5rem;
  font-weight: 300;
  letter-spacing: 0.5rem;
}

.subtitle {
  font-size: 1.2rem;
  opacity: 0.8;
  font-weight: 300;
}

.content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3rem;
}

.poem {
  font-size: 1.2rem;
  line-height: 2;
  font-weight: 300;
  animation: fadeIn 2s ease 0.5s both;
}

.poem small {
  display: block;
  margin-top: 1rem;
  opacity: 0.7;
}

.memorial {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 2rem 0;
  animation: fadeIn 2s ease 1s both;
}

.candle {
  width: 30px;
  height: 100px;
  background: linear-gradient(to top, #f5f5f5, #e0e0e0);
  border-radius: 10px 10px 0 0;
  position: relative;
  margin-bottom: 2rem;
}

.candle::before {
  content: "";
  position: absolute;
  top: -20px;
  left: 10px;
  width: 10px;
  height: 20px;
  background: #ffc107;
  border-radius: 50% 50% 0 0;
  box-shadow: 0 0 20px #ffc107, 0 0 40px #ff9800;
  animation: flicker 1s ease-in-out infinite alternate;
}

@keyframes flicker {
  0%, 100% {
    transform: scaleY(1);
    opacity: 1;
  }
  50% {
    transform: scaleY(0.8);
    opacity: 0.8;
  }
}

.flower-wrapper {
  margin: 2rem 0;
  filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.5));
}

.flower-count {
  margin: 1rem 0;
  font-size: 1.1rem;
  color: #ffc107;
}

.tribute-actions {
  display: flex;
  gap: 1rem;
}

.tribute-btn {
  margin-top: 1rem;
  padding: 0.8rem 2rem;
  background: transparent;
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.tribute-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: white;
}

.footer {
  margin-top: 3rem;
  opacity: 0.6;
  font-size: 0.9rem;
  animation: fadeIn 2s ease 2s both;
}

.copyright {
  margin-top: 0.5rem;
  font-size: 0.8rem;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 音乐控制按钮 */
.music-control {
  position: fixed;
  top: 20px;
  right: 20px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 10;
}

.icon-play, .icon-pause {
  display: inline-block;
  width: 0;
  height: 0;
}

.icon-play {
  border-style: solid;
  border-width: 8px 0 8px 12px;
  border-color: transparent transparent transparent white;
  margin-left: 3px;
}

.icon-pause {
  position: relative;
  width: 12px;
  height: 16px;
}

.icon-pause::before,
.icon-pause::after {
  content: "";
  position: absolute;
  width: 4px;
  height: 16px;
  background: white;
}

.icon-pause::before {
  left: 2px;
}

.icon-pause::after {
  right: 2px;
}

/* 飞舞的花朵 */
.flying-flower {
  position: absolute;
  width: 20px;
  height: 20px;
  background-color: white;
  border-radius: 50%;
  box-shadow: 
    5px 0 0 -2.5px white,
    -5px 0 0 -2.5px white,
    0 5px 0 -2.5px white,
    0 -5px 0 -2.5px white;
  animation: flyUp 3s ease-out forwards;
  z-index: 5;
}

@keyframes flyUp {
  0% {
    bottom: 10%;
    opacity: 1;
    transform: scale(0.5) rotate(0deg);
  }
  100% {
    bottom: 80%;
    opacity: 0;
    transform: scale(1.5) rotate(360deg);
  }
}

/* 祭奠弹窗 */
.memorial-dialog {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  animation: fadeIn 0.3s ease;
}

.memorial-dialog-content {
  background-color: #1e293b;
  border-radius: 10px;
  width: 90%;
  max-width: 500px;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

.dialog-header {
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.dialog-header h2 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 400;
}

.close-btn {
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.7);
  font-size: 1.5rem;
  cursor: pointer;
}

.dialog-body {
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.dialog-body textarea {
  width: 100%;
  background-color: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 5px;
  padding: 0.8rem;
  color: white;
  resize: none;
  font-family: inherit;
}

.dialog-body textarea:focus {
  outline: none;
  border-color: rgba(255, 255, 255, 0.5);
}

.dialog-flowers {
  display: flex;
  gap: 1rem;
  margin: 1rem 0;
}

.submit-btn {
  background-color: #64748b;
  color: white;
  border: none;
  padding: 0.8rem 2rem;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.submit-btn:hover {
  background-color: #475569;
}

/* 响应式设计 */
@media (max-width: 768px) {
  h1 {
    font-size: 2rem;
  }
  
  .poem {
    font-size: 1rem;
  }
  
  .tribute-actions {
    flex-direction: column;
    gap: 0.5rem;
  }
  
  .tribute-btn {
    padding: 0.6rem 1.5rem;
  }
}
</style>
