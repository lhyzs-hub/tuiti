<script setup lang="ts">
import { ref, computed } from 'vue'

// 用户信用数据
const creditData = ref({
  score: 85,
  level: '优秀',
  orders: 120,
  completed: 115,
  cancelled: 5,
 好评率: 98.5,
  响应速度: 4.8,
  服务质量: 4.9
})

// 信用等级
const creditLevel = computed(() => {
  const score = creditData.value.score
  if (score >= 90) return { text: '卓越', color: '#4CAF50', icon: '🏆' }
  if (score >= 80) return { text: '优秀', color: '#2196F3', icon: '⭐' }
  if (score >= 70) return { text: '良好', color: '#FF9800', icon: '👍' }
  if (score >= 60) return { text: '及格', color: '#FFC107', icon: '✅' }
  return { text: '待提升', color: '#F44336', icon: '📈' }
})

// 信用历史
const creditHistory = ref([
  {
    id: 1,
    type: 'increase',
    title: '完成订单',
    desc: '按时完成跑腿订单',
    score: '+5',
    time: '2024-06-15 14:30'
  },
  {
    id: 2,
    type: 'increase',
    title: '获得好评',
    desc: '用户给予5星好评',
    score: '+3',
    time: '2024-06-14 10:20'
  },
  {
    id: 3,
    type: 'decrease',
    title: '取消订单',
    desc: '用户主动取消订单',
    score: '-2',
    time: '2024-06-13 16:45'
  },
  {
    id: 4,
    type: 'increase',
    title: '完成订单',
    desc: '按时完成跑腿订单',
    score: '+5',
    time: '2024-06-12 09:15'
  }
])

// 信用权益
const creditBenefits = ref([
  {
    id: 1,
    title: '免单券',
    desc: '信用分80+可领取',
    icon: '🎁',
    required: 80
  },
  {
    id: 2,
    title: '优先派单',
    desc: '信用分90+可享受',
    icon: '⚡',
    required: 90
  },
  {
    id: 3,
    title: '专属客服',
    desc: '信用分95+可享受',
    icon: '👑',
    required: 95
  },
  {
    id: 4,
    title: '生日特权',
    desc: '信用分70+可享受',
    icon: '🎂',
    required: 70
  }
])

// 计算权益是否解锁（基于实际信用分）
const isBenefitUnlocked = (benefit: typeof creditBenefits.value[0]) => {
  return creditData.value.score >= benefit.required
}

// 获取权益解锁状态文本
const getBenefitStatus = (benefit: typeof creditBenefits.value[0]) => {
  if (creditData.value.score >= benefit.required) {
    return '已解锁'
  }
  return `${benefit.required}分解锁`
}

// 查看详情
const handleViewDetail = (item: typeof creditHistory.value[0]) => {
  uni.showModal({
    title: item.title,
    content: `${item.desc}\n时间: ${item.time}\n分数变化: ${item.score}`,
    showCancel: false
  })
}

// 领取权益
const handleClaimBenefit = (benefit: typeof creditBenefits.value[0]) => {
  if (isBenefitUnlocked(benefit)) {
    uni.showToast({
      title: '领取成功',
      icon: 'success'
    })
  } else {
    uni.showToast({
      title: `需要${benefit.required}分才能解锁`,
      icon: 'none'
    })
  }
}
</script>

<template>
  <view class="container">
    <!-- 信用分数卡片 -->
    <view class="credit-card">
      <view class="credit-header">
        <text class="credit-title">我的信用分</text>
        <view class="credit-level-badge" :style="{ backgroundColor: creditLevel.color }">
          <text class="level-icon">{{ creditLevel.icon }}</text>
          <text class="level-text">{{ creditLevel.text }}</text>
        </view>
      </view>

      <view class="credit-score">
        <text class="score-value">{{ creditData.score }}</text>
        <text class="score-label">分</text>
      </view>

      <view class="credit-progress">
        <view class="progress-bar">
          <view
            class="progress-fill"
            :style="{ width: `${creditData.score}%`, backgroundColor: creditLevel.color }"
          />
        </view>
        <text class="progress-text">{{ creditData.score }}/100</text>
      </view>
    </view>

    <!-- 统计数据 -->
    <view class="stats-section">
      <view class="stats-grid">
        <view class="stat-item">
          <text class="stat-value">{{ creditData.orders }}</text>
          <text class="stat-label">总订单</text>
        </view>
        <view class="stat-divider" />
        <view class="stat-item">
          <text class="stat-value">{{ creditData.completed }}</text>
          <text class="stat-label">已完成</text>
        </view>
        <view class="stat-divider" />
        <view class="stat-item">
          <text class="stat-value">{{ creditData.cancelled }}</text>
          <text class="stat-label">已取消</text>
        </view>
      </view>
    </view>

    <!-- 评价维度 -->
    <view class="dimensions-section">
      <view class="section-title">
        <text class="title-text">评价维度</text>
      </view>

      <view class="dimension-list">
        <view class="dimension-item">
          <view class="dimension-info">
            <text class="dimension-name">好评率</text>
            <text class="dimension-value">{{ creditData.好评率 }}%</text>
          </view>
          <view class="dimension-bar">
            <view
              class="dimension-fill"
              :style="{ width: `${creditData.好评率}%` }"
            />
          </view>
        </view>

        <view class="dimension-item">
          <view class="dimension-info">
            <text class="dimension-name">响应速度</text>
            <text class="dimension-value">{{ creditData.响应速度 }}/5.0</text>
          </view>
          <view class="dimension-bar">
            <view
              class="dimension-fill"
              :style="{ width: `${(creditData.响应速度 / 5) * 100}%` }"
            />
          </view>
        </view>

        <view class="dimension-item">
          <view class="dimension-info">
            <text class="dimension-name">服务质量</text>
            <text class="dimension-value">{{ creditData.服务质量 }}/5.0</text>
          </view>
          <view class="dimension-bar">
            <view
              class="dimension-fill"
              :style="{ width: `${(creditData.服务质量 / 5) * 100}%` }"
            />
          </view>
        </view>
      </view>
    </view>

    <!-- 信用权益 -->
    <view class="benefits-section">
      <view class="section-title">
        <text class="title-text">信用权益</text>
      </view>

      <view class="benefits-list">
        <view
          v-for="benefit in creditBenefits"
          :key="benefit.id"
          class="benefit-item"
          :class="{ unlocked: isBenefitUnlocked(benefit) }"
          @click="handleClaimBenefit(benefit)"
        >
          <view class="benefit-icon">
            <text class="icon-text">{{ benefit.icon }}</text>
          </view>
          <view class="benefit-content">
            <text class="benefit-title">{{ benefit.title }}</text>
            <text class="benefit-desc">{{ benefit.desc }}</text>
          </view>
          <view class="benefit-status">
            <text class="status-text">{{ getBenefitStatus(benefit) }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 信用历史 -->
    <view class="history-section">
      <view class="section-title">
        <text class="title-text">信用历史</text>
      </view>

      <view class="history-list">
        <view
          v-for="item in creditHistory"
          :key="item.id"
          class="history-item"
          @click="handleViewDetail(item)"
        >
          <view class="history-icon" :class="item.type">
            <text class="icon-text">{{ item.type === 'increase' ? '+' : '-' }}</text>
          </view>
          <view class="history-content">
            <text class="history-title">{{ item.title }}</text>
            <text class="history-desc">{{ item.desc }}</text>
            <text class="history-time">{{ item.time }}</text>
          </view>
          <view class="history-score" :class="item.type">
            <text class="score-text">{{ item.score }}</text>
          </view>
        </view>
      </view>
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

.credit-card {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: $border-radius-lg;
  padding: $spacing-lg;
  margin-bottom: $spacing-md;
  color: $bg-white;
  box-shadow: $shadow-md;
}

.credit-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: $spacing-lg;
}

.credit-title {
  font-size: $font-size-lg;
  font-weight: bold;
}

.credit-level-badge {
  display: flex;
  align-items: center;
  gap: $spacing-xs;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: $border-radius-md;
  padding: $spacing-xs $spacing-md;
}

.level-icon {
  font-size: $font-size-md;
}

.level-text {
  font-size: $font-size-sm;
  font-weight: 500;
}

.credit-score {
  text-align: center;
  margin-bottom: $spacing-lg;
}

.score-value {
  font-size: 64px;
  font-weight: bold;
  line-height: 1;
}

.score-label {
  font-size: $font-size-md;
  margin-left: $spacing-xs;
}

.credit-progress {
  display: flex;
  align-items: center;
  gap: $spacing-md;
}

.progress-bar {
  flex: 1;
  height: 8px;
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  border-radius: 4px;
  transition: width 0.3s ease;
}

.progress-text {
  font-size: $font-size-sm;
  opacity: 0.9;
}

.stats-section {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.stats-grid {
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
  color: $primary-color;
}

.stat-label {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.stat-divider {
  width: 1px;
  height: 40px;
  background-color: $border-color;
}

.dimensions-section {
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

.dimension-list {
  display: flex;
  flex-direction: column;
  gap: $spacing-md;
}

.dimension-item {
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.dimension-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.dimension-name {
  font-size: $font-size-sm;
  color: $text-secondary;
}

.dimension-value {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.dimension-bar {
  height: 6px;
  background-color: $bg-color;
  border-radius: 3px;
  overflow: hidden;
}

.dimension-fill {
  height: 100%;
  background: linear-gradient(90deg, $primary-color 0%, $primary-light 100%);
  border-radius: 3px;
  transition: width 0.3s ease;
}

.benefits-section {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.benefits-list {
  display: flex;
  flex-direction: column;
  gap: $spacing-sm;
}

.benefit-item {
  display: flex;
  align-items: center;
  gap: $spacing-md;
  padding: $spacing-md;
  border-radius: $border-radius-md;
  background-color: $bg-color;
  cursor: pointer;
  transition: all 0.3s ease;

  &:active {
    transform: scale(0.98);
  }

  &.unlocked {
    background-color: rgba(76, 175, 80, 0.1);
    border: 1px solid $success-color;
  }
}

.benefit-icon {
  width: 40px;
  height: 40px;
  background-color: $bg-white;
  border-radius: $border-radius-md;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-text {
  font-size: $font-size-lg;
}

.benefit-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.benefit-title {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.benefit-desc {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.benefit-status {
  padding: $spacing-xs $spacing-md;
  border-radius: $border-radius-md;
  background-color: $bg-white;
}

.status-text {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.history-section {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  box-shadow: $shadow-sm;
}

.history-list {
  display: flex;
  flex-direction: column;
  gap: $spacing-sm;
}

.history-item {
  display: flex;
  align-items: center;
  gap: $spacing-md;
  padding: $spacing-md;
  border-radius: $border-radius-md;
  background-color: $bg-color;
  cursor: pointer;
  transition: all 0.3s ease;

  &:active {
    transform: scale(0.98);
  }
}

.history-icon {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;

  &.increase {
    background-color: rgba(76, 175, 80, 0.2);
  }

  &.decrease {
    background-color: rgba(244, 67, 54, 0.2);
  }
}

.history-icon .icon-text {
  font-size: $font-size-md;
  font-weight: bold;

  .increase & {
    color: $success-color;
  }

  .decrease & {
    color: $danger-color;
  }
}

.history-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.history-title {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.history-desc {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.history-time {
  font-size: $font-size-xs;
  color: $text-tertiary;
}

.history-score {
  padding: $spacing-xs $spacing-md;
  border-radius: $border-radius-md;
  font-weight: bold;

  &.increase {
    background-color: rgba(76, 175, 80, 0.2);
    color: $success-color;
  }

  &.decrease {
    background-color: rgba(244, 67, 54, 0.2);
    color: $danger-color;
  }
}

.score-text {
  font-size: $font-size-md;
}
</style>