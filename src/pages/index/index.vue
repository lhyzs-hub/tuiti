<script setup lang="ts">
import { ref } from 'vue'
import { showToast, navigateTo } from '@dcloudio/uni-app'

// 搜索关键词
const searchValue = ref('')

// 服务分类数据
const services = [
  {
    id: 1,
    name: '打印材料',
    icon: '🖨️',
    bgColor: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)'
  },
  {
    id: 2,
    name: '取外卖',
    icon: '🍔',
    bgColor: 'linear-gradient(135deg, #f093fb 0%, #f5576c 100%)'
  },
  {
    id: 3,
    name: '搬东西',
    icon: '📦',
    bgColor: 'linear-gradient(135deg, #4facfe 0%, #00f2fe 100%)'
  },
  {
    id: 4,
    name: '代购物',
    icon: '🛒',
    bgColor: 'linear-gradient(135deg, #43e97b 0%, #38f9d7 100%)'
  },
  {
    id: 5,
    name: '送文件',
    icon: '📄',
    bgColor: 'linear-gradient(135deg, #fa709a 0%, #fee140 100%)'
  },
  {
    id: 6,
    name: '其他',
    icon: '✨',
    bgColor: 'linear-gradient(135deg, #a18cd1 0%, #fbc2eb 100%)'
  }
]

// 轮播图数据
const banners = [
  {
    id: 1,
    title: '新人专享',
    desc: '首单立减5元',
    btnText: '立即体验',
    icon: '🎁',
    bgColor: 'linear-gradient(135deg, #1E88E5 0%, #42A5F5 100%)'
  },
  {
    id: 2,
    title: '限时优惠',
    desc: '跑腿服务8折起',
    btnText: '查看详情',
    icon: '🔥',
    bgColor: 'linear-gradient(135deg, #FF9800 0%, #FFC107 100%)'
  },
  {
    id: 3,
    title: '信用有礼',
    desc: '信用分兑换好礼',
    btnText: '去兑换',
    icon: '⭐',
    bgColor: 'linear-gradient(135deg, #4CAF50 0%, #81C784 100%)'
  }
]

// 快捷操作数据
const quickActions = [
  {
    id: 1,
    name: '紧急跑腿',
    desc: '30分钟送达',
    icon: '⚡',
    bgColor: 'linear-gradient(135deg, #F44336 0%, #FF5252 100%)'
  },
  {
    id: 2,
    name: '预约服务',
    desc: '指定时间送达',
    icon: '📅',
    bgColor: 'linear-gradient(135deg, #2196F3 0%, #42A5F5 100%)'
  },
  {
    id: 3,
    name: '多人拼单',
    desc: '更优惠的价格',
    icon: '👥',
    bgColor: 'linear-gradient(135deg, #9C27B0 0%, #BA68C8 100%)'
  },
  {
    id: 4,
    name: '企业服务',
    desc: '专属商务通道',
    icon: '💼',
    bgColor: 'linear-gradient(135deg, #00BCD4 0%, #26C6DA 100%)'
  }
]

// 统计数据
const stats = {
  orders: '100万+',
  runners: '5000+',
  satisfaction: '98.5'
}

// 搜索功能
const handleSearch = () => {
  if (searchValue.value) {
    showToast({
      title: `搜索: ${searchValue.value}`,
      icon: 'none'
    })
  }
}

// 服务点击处理
const handleServiceClick = (service: typeof services[0]) => {
  console.log('点击了服务:', service.name)

  try {
    // 跳转到发布页面，并传递服务类型
    navigateTo({
      url: `/pages/publish/index?type=${encodeURIComponent(service.name)}`
    })
  } catch (error) {
    console.error('导航失败:', error)
    showToast({
      title: '页面跳转失败',
      icon: 'none'
    })
  }
}

// 快捷操作点击处理
const handleQuickAction = (action: typeof quickActions[0]) => {
  showToast({
    title: action.name,
    icon: 'none'
  })
}
</script>

<template>
  <view class="container">
    <!-- 头部区域 -->
    <view class="header">
      <view class="logo-section">
        <view class="logo">
          <view class="logo-icon">
            <text class="logo-text">腿</text>
          </view>
          <text class="logo-title">腿替</text>
        </view>
        <text class="logo-subtitle">您的专属跑腿服务</text>
      </view>

      <view class="search-bar">
        <view class="search-input-wrap">
          <text class="search-icon">🔍</text>
          <input
            v-model="searchValue"
            class="search-input"
            placeholder="输入您需要的服务"
            @confirm="handleSearch"
          />
        </view>
        <view class="search-btn" @click="handleSearch">
          <text class="search-btn-text">搜索</text>
        </view>
      </view>
    </view>

    <!-- 服务分类 -->
    <view class="service-grid">
      <view class="service-title">
        <text class="title-text">服务分类</text>
        <text class="title-more">查看全部 →</text>
      </view>

      <view class="grid">
        <view
          v-for="service in services"
          :key="service.id"
          class="grid-item"
          :data-service="service.name"
          :data-id="service.id"
          @click="handleServiceClick(service)"
        >
          <view class="service-icon" :style="{ background: service.bgColor }">
            <text class="icon-text">{{ service.icon }}</text>
          </view>
          <text class="service-name">{{ service.name }}</text>
        </view>
      </view>
    </view>

    <!-- 轮播图 -->
    <view class="banner-section">
      <swiper
        class="banner-swiper"
        indicator-dots
        autoplay
        circular
      >
        <swiper-item
          v-for="banner in banners"
          :key="banner.id"
        >
          <view class="banner-item" :style="{ background: banner.bgColor }">
            <view class="banner-content">
              <text class="banner-title">{{ banner.title }}</text>
              <text class="banner-desc">{{ banner.desc }}</text>
              <view class="banner-btn">
                <text class="banner-btn-text">{{ banner.btnText }}</text>
              </view>
            </view>
            <view class="banner-image">
              <text class="banner-icon">{{ banner.icon }}</text>
            </view>
          </view>
        </swiper-item>
      </swiper>
    </view>

    <!-- 快捷操作 -->
    <view class="quick-actions">
      <view class="action-title">
        <text class="title-text">快捷发布</text>
      </view>

      <view class="action-list">
        <view
          v-for="action in quickActions"
          :key="action.id"
          class="action-item"
          @click="handleQuickAction(action)"
        >
          <view class="action-icon-wrap" :style="{ background: action.bgColor }">
            <text class="action-icon">{{ action.icon }}</text>
          </view>
          <text class="action-name">{{ action.name }}</text>
          <text class="action-desc">{{ action.desc }}</text>
        </view>
      </view>
    </view>

    <!-- 统计数据 -->
    <view class="stats-section">
      <view class="stats-card">
        <view class="stats-bg" />
        <view class="stats-content">
          <view class="stat-item">
            <text class="stat-value">{{ stats.orders }}</text>
            <text class="stat-label">累计订单</text>
          </view>
          <view class="stat-divider" />
          <view class="stat-item">
            <text class="stat-value">{{ stats.runners }}</text>
            <text class="stat-label">在线跑腿员</text>
          </view>
          <view class="stat-divider" />
          <view class="stat-item">
            <text class="stat-value">{{ stats.satisfaction }}%</text>
            <text class="stat-label">满意度</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 底部占位 -->
    <view class="footer-space" />
  </view>
</template>

<style lang="scss" scoped>
@import "@/styles/variables.scss";

.container {
  min-height: 100vh;
  background-color: $bg-color;
  padding-bottom: 100px;
}

.header {
  padding: $spacing-md;
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: 0 0 $border-radius-xl $border-radius-xl;
  box-shadow: $shadow-md;
}

.logo-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: $spacing-lg;
}

.logo {
  display: flex;
  align-items: center;
  gap: $spacing-xs;
  margin-bottom: $spacing-xs;
}

.logo-icon {
  width: 40px;
  height: 40px;
  background-color: $bg-white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: $font-size-lg;
  color: $primary-color;
}

.logo-title {
  font-size: $font-size-xl;
  font-weight: bold;
  color: $bg-white;
}

.logo-subtitle {
  font-size: $font-size-sm;
  color: rgba(255, 255, 255, 0.9);
}

.search-bar {
  display: flex;
  gap: $spacing-sm;
}

.search-input-wrap {
  flex: 1;
  display: flex;
  align-items: center;
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-sm $spacing-md;
  gap: $spacing-sm;
}

.search-icon {
  font-size: $font-size-md;
}

.search-input {
  flex: 1;
  border: none;
  outline: none;
  font-size: $font-size-sm;
}

.search-btn {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-sm $spacing-lg;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search-btn-text {
  color: $primary-color;
  font-weight: 500;
}

.service-grid {
  padding: $spacing-md;
}

.service-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: $spacing-md;
}

.title-text {
  font-size: $font-size-lg;
  font-weight: bold;
  color: $text-primary;
}

.title-more {
  font-size: $font-size-sm;
  color: $primary-color;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: $spacing-md;
}

.grid-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-xs;
  cursor: pointer;
  transition: transform 0.2s ease;

  &:active {
    transform: scale(0.95);
  }
}

.service-icon {
  width: 60px;
  height: 60px;
  border-radius: $border-radius-lg;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: $shadow-sm;
}

.icon-text {
  font-size: $font-size-xl;
}

.service-name {
  font-size: $font-size-sm;
  color: $text-secondary;
}

.banner-section {
  padding: $spacing-md;
}

.banner-swiper {
  height: 160px;
  border-radius: $border-radius-lg;
  overflow: hidden;
}

.banner-item {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: $spacing-lg;
  color: $bg-white;
}

.banner-content {
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.banner-title {
  font-size: $font-size-lg;
  font-weight: bold;
}

.banner-desc {
  font-size: $font-size-sm;
  opacity: 0.9;
}

.banner-btn {
  background-color: $bg-white;
  border-radius: $border-radius-md;
  padding: $spacing-xs $spacing-md;
  margin-top: $spacing-xs;
  display: inline-block;
}

.banner-btn-text {
  color: $primary-color;
  font-size: $font-size-sm;
  font-weight: 500;
}

.banner-image {
  font-size: 48px;
}

.banner-icon {
  font-size: 48px;
}

.quick-actions {
  padding: $spacing-md;
}

.action-title {
  margin-bottom: $spacing-md;
}

.action-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: $spacing-md;
}

.action-item {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  display: flex;
  align-items: center;
  gap: $spacing-sm;
  box-shadow: $shadow-sm;
  cursor: pointer;
  transition: transform 0.2s ease;

  &:active {
    transform: scale(0.95);
  }
}

.action-icon-wrap {
  width: 40px;
  height: 40px;
  border-radius: $border-radius-md;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-icon {
  font-size: $font-size-lg;
}

.action-name {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.action-desc {
  font-size: $font-size-xs;
  color: $text-tertiary;
}

.stats-section {
  padding: $spacing-md;
}

.stats-card {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: $border-radius-lg;
  padding: $spacing-lg;
  position: relative;
  overflow: hidden;
}

.stats-bg {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="rgba(255,255,255,0.1)"/></svg>');
  background-size: 200px;
  background-position: center;
}

.stats-content {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-around;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-xs;
}

.stat-value {
  font-size: $font-size-xl;
  font-weight: bold;
  color: $bg-white;
}

.stat-label {
  font-size: $font-size-xs;
  color: rgba(255, 255, 255, 0.9);
}

.stat-divider {
  width: 1px;
  height: 40px;
  background-color: rgba(255, 255, 255, 0.3);
}

.footer-space {
  height: 50px;
}
</style>