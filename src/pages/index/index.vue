<script setup lang="ts">
import { ref } from 'vue'

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
    uni.showToast({
      title: `搜索: ${searchValue.value}`,
      icon: 'none'
    })
  }
}

// 服务点击处理
const handleServiceClick = (service: typeof services[0]) => {
  // 先显示Toast确认点击事件被触发
  uni.showToast({
    title: `选择服务: ${service.name}`,
    icon: 'none',
    duration: 1500
  })
  
  // 使用本地存储传递服务类型（因为publish是tabBar页面，switchTab不能传参）
  uni.setStorageSync('selectedServiceType', service.name)
  
  // 使用switchTab跳转到发布页面（因为publish在tabBar中）
  setTimeout(() => {
    uni.switchTab({
      url: '/pages/publish/index',
      success: () => {
        console.log('跳转成功')
      },
      fail: (err) => {
        console.error('跳转失败:', err)
        uni.showToast({
          title: `跳转失败: ${err.errMsg || '未知错误'}`,
          icon: 'none',
          duration: 2000
        })
      }
    })
  }, 800)
}

// 快捷操作点击处理
const handleQuickAction = (action: typeof quickActions[0]) => {
  const serviceMap: Record<string, string> = {
    '紧急跑腿': '取外卖',
    '预约服务': '送文件',
    '多人拼单': '代购物',
    '企业服务': '其他'
  }
  const serviceType = serviceMap[action.name] || '其他'

  try {
    uni.navigateTo({
      url: `/pages/publish/index?type=${encodeURIComponent(serviceType)}`
    })
  } catch (error) {
    uni.showToast({
      title: '页面跳转失败，请重试',
      icon: 'none'
    })
  }
}

// 轮播图点击处理
const handleBannerClick = (banner: typeof banners[0]) => {
  uni.showToast({
    title: banner.title,
    icon: 'none'
  })
  if (banner.id === 1) {
    uni.showToast({
      title: '新人专享优惠正在准备中',
      icon: 'none'
    })
  } else if (banner.id === 2) {
    uni.showToast({
      title: '限时优惠活动即将上线',
      icon: 'none'
    })
  } else if (banner.id === 3) {
    uni.showToast({
      title: '查看信用权益',
      icon: 'none'
    })
  }
}

// 查看全部服务
const handleViewAllServices = () => {
  uni.showToast({
    title: '全部服务正在整理中',
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
        <text class="title-more" @click="handleViewAllServices">查看全部 →</text>
      </view>

      <view class="grid">
        <view
          v-for="service in services"
          :key="service.id"
          class="grid-item"
          :data-service="service.name"
          :data-id="service.id"
          @click="handleServiceClick(service)"
          @tap="handleServiceClick(service)"
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
          <view class="banner-item" :style="{ background: banner.bgColor }" @click="handleBannerClick(banner)">
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

    <view class="footer-space" />
  </view>
</template>

<style lang="scss" scoped>
@import "@/styles/variables.scss";

.container {
  min-height: 100vh;
  background: $bg-color;
  padding-bottom: calc(env(safe-area-inset-bottom) + 100rpx);
}

.header {
  background: linear-gradient(135deg, $primary-color 0%, $secondary-color 100%);
  padding: 60rpx 32rpx 40rpx;
  border-radius: 0 0 48rpx 48rpx;
}

.logo-section {
  margin-bottom: 32rpx;
}

.logo {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.logo-icon {
  width: 80rpx;
  height: 80rpx;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.logo-text {
  font-size: 40rpx;
  font-weight: bold;
  color: #fff;
}

.logo-title {
  font-size: 40rpx;
  font-weight: bold;
  color: #fff;
}

.logo-subtitle {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.8);
  margin-top: 8rpx;
  display: block;
}

.search-bar {
  display: flex;
  gap: 16rpx;
  align-items: center;
}

.search-input-wrap {
  flex: 1;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 40rpx;
  padding: 20rpx 24rpx;
  display: flex;
  align-items: center;
  gap: 12rpx;
}

.search-icon {
  font-size: 32rpx;
}

.search-input {
  flex: 1;
  font-size: 28rpx;
  color: #333;
}

.search-btn {
  background: rgba(255, 255, 255, 0.95);
  padding: 20rpx 32rpx;
  border-radius: 40rpx;
}

.search-btn-text {
  font-size: 28rpx;
  font-weight: 500;
  color: $primary-color;
}

.service-grid {
  padding: 32rpx;
}

.service-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24rpx;
}

.title-text {
  font-size: 32rpx;
  font-weight: 600;
  color: #333;
}

.title-more {
  font-size: 26rpx;
  color: $primary-color;
}

.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24rpx;
}

.grid-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12rpx;
  padding: 16rpx;
  border-radius: 16rpx;
  transition: all 0.2s ease;
  cursor: pointer;
}

.grid-item:active {
  transform: scale(0.95);
  background: rgba(30, 136, 229, 0.1);
}

.service-icon {
  width: 100rpx;
  height: 100rpx;
  border-radius: 24rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-text {
  font-size: 40rpx;
}

.service-name {
  font-size: 24rpx;
  color: #333;
}

.banner-section {
  padding: 0 32rpx;
  margin-bottom: 32rpx;
}

.banner-swiper {
  height: 200rpx;
  border-radius: 24rpx;
  overflow: hidden;
}

.banner-item {
  height: 100%;
  padding: 24rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.banner-content {
  flex: 1;
}

.banner-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #fff;
  display: block;
  margin-bottom: 8rpx;
}

.banner-desc {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.9);
  display: block;
  margin-bottom: 16rpx;
}

.banner-btn {
  background: rgba(255, 255, 255, 0.2);
  padding: 12rpx 24rpx;
  border-radius: 24rpx;
  display: inline-block;
}

.banner-btn-text {
  font-size: 24rpx;
  color: #fff;
}

.banner-image {
  width: 120rpx;
  height: 120rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.banner-icon {
  font-size: 60rpx;
}

.quick-actions {
  padding: 0 32rpx 32rpx;
}

.action-title {
  margin-bottom: 24rpx;
}

.action-list {
  display: flex;
  gap: 16rpx;
}

.action-item {
  flex: 1;
  background: #fff;
  border-radius: 20rpx;
  padding: 20rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.05);
}

.action-icon-wrap {
  width: 72rpx;
  height: 72rpx;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-icon {
  font-size: 36rpx;
}

.action-name {
  font-size: 26rpx;
  font-weight: 500;
  color: #333;
}

.action-desc {
  font-size: 20rpx;
  color: #999;
}

.stats-section {
  padding: 0 32rpx;
}

.stats-card {
  background: linear-gradient(135deg, $primary-color 0%, $secondary-color 100%);
  border-radius: 24rpx;
  padding: 32rpx;
  position: relative;
  overflow: hidden;
}

.stats-bg {
  position: absolute;
  top: -50%;
  right: -20%;
  width: 200rpx;
  height: 200rpx;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 50%;
}

.stats-content {
  display: flex;
  justify-content: space-around;
  align-items: center;
  position: relative;
  z-index: 1;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8rpx;
}

.stat-value {
  font-size: 40rpx;
  font-weight: bold;
  color: #fff;
}

.stat-label {
  font-size: 24rpx;
  color: rgba(255, 255, 255, 0.8);
}

.stat-divider {
  width: 2rpx;
  height: 60rpx;
  background: rgba(255, 255, 255, 0.3);
}

.footer-space {
  height: 100rpx;
}
</style>