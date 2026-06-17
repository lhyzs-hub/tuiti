<script setup lang="ts">
import { ref } from 'vue'

// 用户信息
const userInfo = ref({
  avatar: '👤',
  nickname: '用户001',
  phone: '138****8000',
  creditScore: 85,
  orders: 120,
  balance: 150.00
})

// 订单统计
const orderStats = ref({
  pending: 2,
  inProgress: 1,
  completed: 115,
  cancelled: 5
})

// 功能列表
const menuItems = ref([
  {
    id: 1,
    icon: '📋',
    title: '我的订单',
    desc: '查看所有订单记录',
    path: '/pages/orders/index'
  },
  {
    id: 2,
    icon: '💰',
    title: '我的钱包',
    desc: '余额、优惠券、积分',
    path: '/pages/wallet/index'
  },
  {
    id: 3,
    icon: '📍',
    title: '地址管理',
    desc: '管理常用地址',
    path: '/pages/address/index'
  },
  {
    id: 4,
    icon: '⭐',
    title: '我的评价',
    desc: '查看收到的评价',
    path: '/pages/reviews/index'
  },
  {
    id: 5,
    icon: '🎁',
    title: '邀请好友',
    desc: '邀请好友获得奖励',
    path: '/pages/invite/index'
  },
  {
    id: 6,
    icon: '⚙️',
    title: '设置',
    desc: '账号、通知、隐私',
    path: '/pages/settings/index'
  },
  {
    id: 7,
    icon: '💬',
    title: '帮助与反馈',
    desc: '常见问题、意见反馈',
    path: '/pages/help/index'
  },
  {
    id: 8,
    icon: 'ℹ️',
    title: '关于我们',
    desc: '版本信息、公司介绍',
    path: '/pages/about/index'
  }
])

// 跳转页面
const handleNavigate = (item: typeof menuItems.value[0]) => {
  uni.showToast({
    title: `跳转到${item.title}`,
    icon: 'none'
  })
}

// 编辑资料
const handleEditProfile = () => {
  uni.showToast({
    title: '编辑个人资料',
    icon: 'none'
  })
}

// 退出登录
const handleLogout = () => {
  uni.showModal({
    title: '退出登录',
    content: '确定要退出登录吗？',
    success: (res) => {
      if (res.confirm) {
        uni.showToast({
          title: '已退出登录',
          icon: 'success'
        })
      }
    }
  })
}
</script>

<template>
  <view class="container">
    <!-- 用户信息卡片 -->
    <view class="user-card">
      <view class="user-header">
        <view class="user-avatar" @click="handleEditProfile">
          <text class="avatar-text">{{ userInfo.avatar }}</text>
        </view>
        <view class="user-info">
          <text class="user-name">{{ userInfo.nickname }}</text>
          <text class="user-phone">{{ userInfo.phone }}</text>
        </view>
        <view class="edit-btn" @click="handleEditProfile">
          <text class="edit-text">编辑</text>
        </view>
      </view>

      <view class="user-stats">
        <view class="stat-item">
          <text class="stat-value">{{ userInfo.orders }}</text>
          <text class="stat-label">订单</text>
        </view>
        <view class="stat-divider" />
        <view class="stat-item">
          <text class="stat-value">{{ userInfo.creditScore }}</text>
          <text class="stat-label">信用分</text>
        </view>
        <view class="stat-divider" />
        <view class="stat-item">
          <text class="stat-value">¥{{ userInfo.balance.toFixed(2) }}</text>
          <text class="stat-label">余额</text>
        </view>
      </view>
    </view>

    <!-- 订单状态 -->
    <view class="order-status">
      <view class="status-title">
        <text class="title-text">订单状态</text>
      </view>

      <view class="status-grid">
        <view class="status-item">
          <view class="status-icon pending">
            <text class="icon-text">📋</text>
          </view>
          <text class="status-label">待接单</text>
          <text class="status-count">{{ orderStats.pending }}</text>
        </view>

        <view class="status-item">
          <view class="status-icon in-progress">
            <text class="icon-text">🚚</text>
          </view>
          <text class="status-label">进行中</text>
          <text class="status-count">{{ orderStats.inProgress }}</text>
        </view>

        <view class="status-item">
          <view class="status-icon completed">
            <text class="icon-text">✅</text>
          </view>
          <text class="status-label">已完成</text>
          <text class="status-count">{{ orderStats.completed }}</text>
        </view>

        <view class="status-item">
          <view class="status-icon cancelled">
            <text class="icon-text">❌</text>
          </view>
          <text class="status-label">已取消</text>
          <text class="status-count">{{ orderStats.cancelled }}</text>
        </view>
      </view>
    </view>

    <!-- 功能菜单 -->
    <view class="menu-section">
      <view class="menu-list">
        <view
          v-for="item in menuItems"
          :key="item.id"
          class="menu-item"
          @click="handleNavigate(item)"
        >
          <view class="menu-icon">
            <text class="icon-text">{{ item.icon }}</text>
          </view>
          <view class="menu-content">
            <text class="menu-title">{{ item.title }}</text>
            <text class="menu-desc">{{ item.desc }}</text>
          </view>
          <view class="menu-arrow">
            <text class="arrow-text">›</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 退出登录 -->
    <view class="logout-section">
      <button class="btn btn-logout" @click="handleLogout">
        退出登录
      </button>
    </view>
  </view>
</template>

<style lang="scss" scoped>
@import "@/styles/variables.scss";

.container {
  min-height: 100vh;
  background-color: $bg-color;
  padding-bottom: 100px;
}

.user-card {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: 0 0 $border-radius-xl $border-radius-xl;
  padding: $spacing-lg;
  margin-bottom: $spacing-md;
  color: $bg-white;
  box-shadow: $shadow-md;
}

.user-header {
  display: flex;
  align-items: center;
  gap: $spacing-md;
  margin-bottom: $spacing-lg;
}

.user-avatar {
  width: 60px;
  height: 60px;
  background-color: $bg-white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: transform 0.3s ease;

  &:active {
    transform: scale(0.95);
  }
}

.avatar-text {
  font-size: 32px;
}

.user-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.user-name {
  font-size: $font-size-lg;
  font-weight: bold;
}

.user-phone {
  font-size: $font-size-sm;
  opacity: 0.9;
}

.edit-btn {
  padding: $spacing-xs $spacing-md;
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: $border-radius-md;
  cursor: pointer;
  transition: all 0.3s ease;

  &:active {
    transform: scale(0.95);
  }
}

.edit-text {
  font-size: $font-size-sm;
  font-weight: 500;
}

.user-stats {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding: $spacing-md 0;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
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
}

.stat-label {
  font-size: $font-size-xs;
  opacity: 0.9;
}

.stat-divider {
  width: 1px;
  height: 40px;
  background-color: rgba(255, 255, 255, 0.3);
}

.order-status {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.status-title {
  margin-bottom: $spacing-md;
  padding-bottom: $spacing-sm;
  border-bottom: 1px solid $border-color;
}

.title-text {
  font-size: $font-size-lg;
  font-weight: bold;
  color: $text-primary;
}

.status-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: $spacing-md;
}

.status-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-xs;
  cursor: pointer;
  transition: transform 0.3s ease;

  &:active {
    transform: scale(0.95);
  }
}

.status-icon {
  width: 50px;
  height: 50px;
  border-radius: $border-radius-lg;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: $spacing-xs;

  &.pending {
    background-color: rgba(255, 193, 7, 0.1);
  }

  &.in-progress {
    background-color: rgba(33, 150, 243, 0.1);
  }

  &.completed {
    background-color: rgba(76, 175, 80, 0.1);
  }

  &.cancelled {
    background-color: rgba(244, 67, 54, 0.1);
  }
}

.icon-text {
  font-size: $font-size-xl;
}

.status-label {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.status-count {
  font-size: $font-size-md;
  font-weight: bold;
  color: $primary-color;
}

.menu-section {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.menu-list {
  display: flex;
  flex-direction: column;
}

.menu-item {
  display: flex;
  align-items: center;
  gap: $spacing-md;
  padding: $spacing-md 0;
  border-bottom: 1px solid $bg-color;
  cursor: pointer;
  transition: background-color 0.3s ease;

  &:last-child {
    border-bottom: none;
  }

  &:active {
    background-color: $bg-color;
  }
}

.menu-icon {
  width: 40px;
  height: 40px;
  background-color: $bg-color;
  border-radius: $border-radius-md;
  display: flex;
  align-items: center;
  justify-content: center;
}

.menu-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.menu-title {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.menu-desc {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.menu-arrow {
  padding: $spacing-sm;
}

.arrow-text {
  font-size: $font-size-xl;
  color: $text-tertiary;
}

.logout-section {
  padding: $spacing-md;
}

.btn {
  width: 100%;
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

  &.logout {
    background-color: $bg-white;
    color: $danger-color;
    border: 1px solid $danger-color;
  }
}
</style>