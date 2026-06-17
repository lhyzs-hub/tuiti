<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

// 模拟位置数据
const userLocation = ref({ lat: 30.2741, lng: 120.1551 })
const runnerLocation = ref({ lat: 30.2800, lng: 120.1600 })

// 计算距离
const distance = ref(1.2)

// 预计到达时间
const eta = ref(15)

// 订单状态
const orderStatus = ref('进行中')

// 更新位置
let updateTimer: number | null = null

onMounted(() => {
  // 模拟实时位置更新
  updateTimer = setInterval(() => {
    // 模拟跑腿员移动
    runnerLocation.value.lat += (Math.random() - 0.5) * 0.001
    runnerLocation.value.lng += (Math.random() - 0.5) * 0.001

    // 更新距离
    distance.value = Math.max(0.1, distance.value - 0.1)

    // 更新预计到达时间
    eta.value = Math.max(1, Math.ceil(distance.value * 10))

    if (distance.value <= 0.1) {
      orderStatus.value = '已送达'
      if (updateTimer) {
        clearInterval(updateTimer)
      }
    }
  }, 3000)
})

onUnmounted(() => {
  if (updateTimer) {
    clearInterval(updateTimer)
  }
})

// 联系跑腿员
const handleContact = () => {
  uni.showModal({
    title: '联系跑腿员',
    content: '是否拨打跑腿员电话？',
    success: (res) => {
      if (res.confirm) {
        uni.makePhoneCall({
          phoneNumber: '13800138000'
        })
      }
    }
  })
}

// 取消订单
const handleCancel = () => {
  uni.showModal({
    title: '取消订单',
    content: '确定要取消当前订单吗？',
    success: (res) => {
      if (res.confirm) {
        uni.showToast({
          title: '订单已取消',
          icon: 'success'
        })
        setTimeout(() => {
          uni.navigateBack()
        }, 1500)
      }
    }
  })
}
</script>

<template>
  <view class="container">
    <view class="map-section">
      <view class="map-placeholder">
        <text class="map-icon">🗺️</text>
        <text class="map-text">实时地图</text>
      </view>

      <view class="distance-display">
        <view class="distance-item">
          <text class="distance-label">双方距离</text>
          <text class="distance-value">{{ distance.toFixed(1) }}km</text>
        </view>
        <view class="distance-divider" />
        <view class="distance-item">
          <text class="distance-label">预计到达</text>
          <text class="distance-value">{{ eta }}分钟</text>
        </view>
      </view>
    </view>

    <view class="status-section">
      <view class="status-card">
        <view class="status-header">
          <text class="status-title">订单状态</text>
          <view class="status-badge" :class="{ completed: orderStatus === '已送达' }">
            <text class="status-text">{{ orderStatus }}</text>
          </view>
        </view>

        <view class="timeline">
          <view class="timeline-item completed">
            <view class="timeline-dot" />
            <view class="timeline-content">
              <text class="timeline-title">订单已接单</text>
              <text class="timeline-time">10:30</text>
            </view>
          </view>

          <view class="timeline-item" :class="{ completed: orderStatus === '已送达' }">
            <view class="timeline-dot" />
            <view class="timeline-content">
              <text class="timeline-title">跑腿员已出发</text>
              <text class="timeline-time">10:32</text>
            </view>
          </view>

          <view class="timeline-item" :class="{ completed: orderStatus === '已送达' }">
            <view class="timeline-dot" />
            <view class="timeline-content">
              <text class="timeline-title">正在配送中</text>
              <text class="timeline-time">进行中</text>
            </view>
          </view>

          <view class="timeline-item" :class="{ completed: orderStatus === '已送达' }">
            <view class="timeline-dot" />
            <view class="timeline-content">
              <text class="timeline-title">已送达</text>
              <text class="timeline-time">待完成</text>
            </view>
          </view>
        </view>
      </view>
    </view>

    <view class="runner-section">
      <view class="runner-card">
        <view class="runner-info">
          <view class="runner-avatar">
            <text class="avatar-text">跑</text>
          </view>
          <view class="runner-details">
            <text class="runner-name">跑腿员001</text>
            <text class="runner-phone">138****8000</text>
          </view>
        </view>

        <view class="runner-stats">
          <view class="stat-item">
            <text class="stat-value">4.9</text>
            <text class="stat-label">评分</text>
          </view>
          <view class="stat-divider" />
          <view class="stat-item">
            <text class="stat-value">1000+</text>
            <text class="stat-label">订单</text>
          </view>
        </view>
      </view>
    </view>

    <view class="action-section">
      <button class="btn btn-secondary" @click="handleContact">
        联系跑腿员
      </button>
      <button class="btn btn-danger" @click="handleCancel">
        取消订单
      </button>
    </view>
  </view>
</template>

<style lang="scss" scoped>
@import "@/styles/variables.scss";

.container {
  min-height: 100vh;
  background-color: $bg-color;
  padding: $spacing-md;
  padding-bottom: 100px;
}

.map-section {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.map-placeholder {
  height: 200px;
  background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%);
  border-radius: $border-radius-md;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: $spacing-sm;
  margin-bottom: $spacing-md;
}

.map-icon {
  font-size: 48px;
}

.map-text {
  font-size: $font-size-md;
  color: $primary-color;
}

.distance-display {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: $spacing-md 0;
  border-top: 1px solid $bg-color;
}

.distance-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-xs;
}

.distance-label {
  font-size: $font-size-sm;
  color: $text-secondary;
}

.distance-value {
  font-size: $font-size-xl;
  font-weight: bold;
  color: $primary-color;
}

.distance-divider {
  width: 1px;
  height: 40px;
  background-color: $border-color;
}

.status-section {
  margin-bottom: $spacing-md;
}

.status-card {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  box-shadow: $shadow-sm;
}

.status-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: $spacing-lg;
}

.status-title {
  font-size: $font-size-lg;
  font-weight: bold;
  color: $text-primary;
}

.status-badge {
  background-color: $warning-color;
  border-radius: $border-radius-md;
  padding: $spacing-xs $spacing-md;

  &.completed {
    background-color: $success-color;
  }
}

.status-text {
  font-size: $font-size-sm;
  color: $bg-white;
  font-weight: 500;
}

.timeline {
  display: flex;
  flex-direction: column;
  gap: $spacing-lg;
}

.timeline-item {
  display: flex;
  align-items: flex-start;
  gap: $spacing-md;
  position: relative;

  &:not(:last-child)::after {
    content: '';
    position: absolute;
    left: 6px;
    top: 20px;
    bottom: -20px;
    width: 2px;
    background-color: $border-color;
  }

  &.completed {
    .timeline-dot {
      background-color: $success-color;
    }

    .timeline-title {
      color: $text-primary;
    }
  }
}

.timeline-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background-color: $border-color;
  flex-shrink: 0;
}

.timeline-content {
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.timeline-title {
  font-size: $font-size-md;
  color: $text-tertiary;
}

.timeline-time {
  font-size: $font-size-xs;
  color: $text-tertiary;
}

.runner-section {
  margin-bottom: $spacing-md;
}

.runner-card {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  box-shadow: $shadow-sm;
}

.runner-info {
  display: flex;
  align-items: center;
  gap: $spacing-md;
  margin-bottom: $spacing-md;
}

.runner-avatar {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.avatar-text {
  font-size: $font-size-md;
  color: $bg-white;
  font-weight: bold;
}

.runner-details {
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.runner-name {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.runner-phone {
  font-size: $font-size-sm;
  color: $text-secondary;
}

.runner-stats {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: $spacing-md 0;
  border-top: 1px solid $bg-color;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-xs;
}

.stat-value {
  font-size: $font-size-lg;
  font-weight: bold;
  color: $primary-color;
}

.stat-label {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.stat-divider {
  width: 1px;
  height: 30px;
  background-color: $border-color;
}

.action-section {
  display: flex;
  gap: $spacing-md;
}

.btn {
  flex: 1;
  padding: $spacing-md;
  border: none;
  border-radius: $border-radius-lg;
  font-size: $font-size-md;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;

  &:active {
    transform: scale(0.95);
  }

  &.secondary {
    background-color: $bg-white;
    color: $text-primary;
    border: 1px solid $border-color;
  }

  &.danger {
    background-color: $danger-color;
    color: $bg-white;
  }
}
</style>