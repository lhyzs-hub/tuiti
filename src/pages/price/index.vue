<script setup lang="ts">
import { ref, computed } from 'vue'
import { onLoad } from '@dcloudio/uni-app'

// 订单数据
const orderData = ref({
  itemType: '',
  itemDesc: '',
  pickupAddress: '',
  deliveryAddress: '',
  pickupTime: '',
  deliveryTime: '',
  specialRequirements: '',
  contactPhone: '',
  contactName: '',
  price: 0
})

// 距离（模拟）
const distance = ref(3.5)

// 计算详细价格
const priceBreakdown = computed(() => {
  const basePrice = orderData.value.price
  const distanceFee = Math.ceil(distance.value) * 2
  const timeFee = orderData.value.pickupTime ? 3 : 0
  const total = basePrice + distanceFee + timeFee

  return {
    base: basePrice,
    distance: distanceFee,
    time: timeFee,
    total
  }
})

// 页面加载
onLoad((options) => {
  if (options?.data) {
    try {
      orderData.value = JSON.parse(decodeURIComponent(options.data))
    } catch (error) {
      console.error('解析订单数据失败:', error)
    }
  }
})

// 确认下单
const handleConfirm = () => {
  uni.showToast({
    title: '下单成功',
    icon: 'success'
  })

  // 跳转到追踪页面
  setTimeout(() => {
    uni.navigateTo({
      url: '/pages/tracking/index'
    })
  }, 1500)
}

// 返回修改
const handleBack = () => {
  uni.navigateBack()
}
</script>

<template>
  <view class="container">
    <view class="price-card">
      <view class="card-header">
        <text class="header-title">智能定价</text>
        <text class="header-subtitle">基于距离和时间的动态定价</text>
      </view>

      <view class="price-display">
        <text class="price-label">预估总价</text>
        <text class="price-value">¥{{ priceBreakdown.total }}</text>
      </view>

      <view class="price-details">
        <view class="detail-item">
          <text class="detail-label">基础费用</text>
          <text class="detail-value">¥{{ priceBreakdown.base }}</text>
        </view>

        <view class="detail-item">
          <text class="detail-label">距离费用 ({{ distance }}km)</text>
          <text class="detail-value">¥{{ priceBreakdown.distance }}</text>
        </view>

        <view class="detail-item">
          <text class="detail-label">时间加急费</text>
          <text class="detail-value">¥{{ priceBreakdown.time }}</text>
        </view>
      </view>
    </view>

    <view class="order-summary">
      <view class="section-title">
        <text class="title-text">订单摘要</text>
      </view>

      <view class="summary-item">
        <text class="summary-label">服务类型</text>
        <text class="summary-value">{{ orderData.itemType }}</text>
      </view>

      <view class="summary-item">
        <text class="summary-label">物品描述</text>
        <text class="summary-value">{{ orderData.itemDesc }}</text>
      </view>

      <view class="summary-item">
        <text class="summary-label">取货地址</text>
        <text class="summary-value">{{ orderData.pickupAddress }}</text>
      </view>

      <view class="summary-item">
        <text class="summary-label">送达地址</text>
        <text class="summary-value">{{ orderData.deliveryAddress }}</text>
      </view>
    </view>

    <view class="button-group">
      <button class="btn btn-secondary" @click="handleBack">
        返回修改
      </button>
      <button class="btn btn-primary" @click="handleConfirm">
        确认下单
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

.price-card {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: $border-radius-lg;
  padding: $spacing-lg;
  margin-bottom: $spacing-md;
  color: $bg-white;
  box-shadow: $shadow-md;
}

.card-header {
  margin-bottom: $spacing-lg;
}

.header-title {
  font-size: $font-size-xl;
  font-weight: bold;
  display: block;
  margin-bottom: $spacing-xs;
}

.header-subtitle {
  font-size: $font-size-sm;
  opacity: 0.9;
}

.price-display {
  text-align: center;
  margin-bottom: $spacing-lg;
  padding: $spacing-lg 0;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.price-label {
  font-size: $font-size-md;
  display: block;
  margin-bottom: $spacing-sm;
}

.price-value {
  font-size: 48px;
  font-weight: bold;
}

.price-details {
  display: flex;
  flex-direction: column;
  gap: $spacing-sm;
}

.detail-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.detail-label {
  font-size: $font-size-sm;
  opacity: 0.9;
}

.detail-value {
  font-size: $font-size-md;
  font-weight: 500;
}

.order-summary {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.section-title {
  margin-bottom: $spacing-md;
  padding-bottom: $spacing-sm;
  border-bottom: 1px solid $border-color;
}

.title-text {
  font-size: $font-size-lg;
  font-weight: bold;
  color: $text-primary;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: $spacing-sm 0;
  border-bottom: 1px solid $bg-color;

  &:last-child {
    border-bottom: none;
  }
}

.summary-label {
  font-size: $font-size-sm;
  color: $text-secondary;
}

.summary-value {
  font-size: $font-size-sm;
  color: $text-primary;
  text-align: right;
  max-width: 60%;
}

.button-group {
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

  &.primary {
    background-color: $primary-color;
    color: $bg-white;
  }

  &.secondary {
    background-color: $bg-white;
    color: $text-primary;
    border: 1px solid $border-color;
  }
}
</style>