<script setup lang="ts">
import { ref } from 'vue'

// 品牌故事
const brandStory = ref({
  title: '腿替的故事',
  content: '腿替成立于2024年，致力于为用户提供最便捷、最可靠的跑腿服务。我们相信，每一次跑腿都是一次信任的传递，每一次服务都是一次温暖的相遇。'
})

// 品牌价值观
const brandValues = ref([
  {
    id: 1,
    icon: '🤝',
    title: '信任',
    desc: '以诚待人，以信立业'
  },
  {
    id: 2,
    icon: '⚡',
    title: '高效',
    desc: '快速响应，准时送达'
  },
  {
    id: 3,
    icon: '💝',
    title: '用心',
    desc: '用心服务，温暖你我'
  },
  {
    id: 4,
    icon: '🌟',
    title: '卓越',
    desc: '追求卓越，永不止步'
  }
])

// 品牌数据
const brandStats = ref([
  {
    id: 1,
    value: '100万+',
    label: '服务用户'
  },
  {
    id: 2,
    value: '5000+',
    label: '跑腿员'
  },
  {
    id: 3,
    value: '98.5%',
    label: '满意度'
  },
  {
    id: 4,
    value: '30分钟',
    label: '平均响应'
  }
])

// 品牌承诺
const brandPromises = ref([
  {
    id: 1,
    title: '安全保障',
    desc: '所有跑腿员经过严格审核和培训',
    icon: '🛡️'
  },
  {
    id: 2,
    title: '价格透明',
    desc: '明码标价，无隐形消费',
    icon: '💰'
  },
  {
    id: 3,
    title: '售后无忧',
    desc: '7×24小时客服，问题及时解决',
    icon: '🎧'
  },
  {
    id: 4,
    title: '隐私保护',
    desc: '严格保护用户个人信息',
    icon: '🔒'
  }
])

// 联系我们
const contactInfo = ref({
  phone: '400-888-8888',
  email: 'service@tuiti.com',
  address: '浙江省杭州市西湖区',
  wechat: 'tuiti_service'
})

// 联系方式
const handleContact = (type: string) => {
  uni.showToast({
    title: `正在联系${type}`,
    icon: 'none'
  })
}

// 分享品牌
const handleShare = () => {
  uni.share({
    provider: 'weixin',
    scene: 'WXSceneSession',
    type: 0,
    summary: '腿替 - 您的专属跑腿服务',
    success: () => {
      uni.showToast({
        title: '分享成功',
        icon: 'success'
      })
    }
  })
}
</script>

<template>
  <view class="container">
    <!-- 品牌头部 -->
    <view class="brand-header">
      <view class="brand-logo">
        <text class="logo-text">腿</text>
      </view>
      <text class="brand-name">腿替</text>
      <text class="brand-slogan">您的专属跑腿服务</text>
    </view>

    <!-- 品牌故事 -->
    <view class="brand-story">
      <view class="section-title">
        <text class="title-text">品牌故事</text>
      </view>
      <view class="story-content">
        <text class="story-text">{{ brandStory.content }}</text>
      </view>
    </view>

    <!-- 品牌价值观 -->
    <view class="brand-values">
      <view class="section-title">
        <text class="title-text">品牌价值观</text>
      </view>
      <view class="values-grid">
        <view
          v-for="value in brandValues"
          :key="value.id"
          class="value-item"
        >
          <view class="value-icon">
            <text class="icon-text">{{ value.icon }}</text>
          </view>
          <text class="value-title">{{ value.title }}</text>
          <text class="value-desc">{{ value.desc }}</text>
        </view>
      </view>
    </view>

    <!-- 品牌数据 -->
    <view class="brand-stats">
      <view class="stats-grid">
        <view
          v-for="stat in brandStats"
          :key="stat.id"
          class="stat-item"
        >
          <text class="stat-value">{{ stat.value }}</text>
          <text class="stat-label">{{ stat.label }}</text>
        </view>
      </view>
    </view>

    <!-- 品牌承诺 -->
    <view class="brand-promises">
      <view class="section-title">
        <text class="title-text">品牌承诺</text>
      </view>
      <view class="promises-list">
        <view
          v-for="promise in brandPromises"
          :key="promise.id"
          class="promise-item"
        >
          <view class="promise-icon">
            <text class="icon-text">{{ promise.icon }}</text>
          </view>
          <view class="promise-content">
            <text class="promise-title">{{ promise.title }}</text>
            <text class="promise-desc">{{ promise.desc }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 联系我们 -->
    <view class="contact-section">
      <view class="section-title">
        <text class="title-text">联系我们</text>
      </view>
      <view class="contact-list">
        <view class="contact-item" @click="handleContact('客服电话')">
          <view class="contact-icon">
            <text class="icon-text">📞</text>
          </view>
          <view class="contact-content">
            <text class="contact-label">客服电话</text>
            <text class="contact-value">{{ contactInfo.phone }}</text>
          </view>
        </view>

        <view class="contact-item" @click="handleContact('邮箱')">
          <view class="contact-icon">
            <text class="icon-text">📧</text>
          </view>
          <view class="contact-content">
            <text class="contact-label">邮箱</text>
            <text class="contact-value">{{ contactInfo.email }}</text>
          </view>
        </view>

        <view class="contact-item" @click="handleContact('地址')">
          <view class="contact-icon">
            <text class="icon-text">📍</text>
          </view>
          <view class="contact-content">
            <text class="contact-label">地址</text>
            <text class="contact-value">{{ contactInfo.address }}</text>
          </view>
        </view>

        <view class="contact-item" @click="handleContact('微信')">
          <view class="contact-icon">
            <text class="icon-text">💬</text>
          </view>
          <view class="contact-content">
            <text class="contact-label">微信</text>
            <text class="contact-value">{{ contactInfo.wechat }}</text>
          </view>
        </view>
      </view>
    </view>

    <!-- 分享按钮 -->
    <view class="share-section">
      <button class="btn btn-primary" @click="handleShare">
        分享给朋友
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

.brand-header {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: $border-radius-lg;
  padding: $spacing-xl;
  margin-bottom: $spacing-md;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-sm;
  color: $bg-white;
  box-shadow: $shadow-md;
}

.brand-logo {
  width: 80px;
  height: 80px;
  background-color: $bg-white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: $spacing-sm;
}

.logo-text {
  font-size: 40px;
  font-weight: bold;
  color: $primary-color;
}

.brand-name {
  font-size: $font-size-xxl;
  font-weight: bold;
}

.brand-slogan {
  font-size: $font-size-md;
  opacity: 0.9;
}

.brand-story {
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

.story-content {
  padding: $spacing-md 0;
}

.story-text {
  font-size: $font-size-md;
  line-height: 1.8;
  color: $text-secondary;
}

.brand-values {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.values-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: $spacing-md;
}

.value-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: $spacing-xs;
  padding: $spacing-md;
  background-color: $bg-color;
  border-radius: $border-radius-md;
}

.value-icon {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-text {
  font-size: $font-size-xl;
}

.value-title {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.value-desc {
  font-size: $font-size-xs;
  color: $text-secondary;
  text-align: center;
}

.brand-stats {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: $border-radius-lg;
  padding: $spacing-lg;
  margin-bottom: $spacing-md;
  color: $bg-white;
  box-shadow: $shadow-md;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: $spacing-lg;
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
  font-size: $font-size-sm;
  opacity: 0.9;
}

.brand-promises {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.promises-list {
  display: flex;
  flex-direction: column;
  gap: $spacing-md;
}

.promise-item {
  display: flex;
  align-items: center;
  gap: $spacing-md;
  padding: $spacing-md;
  background-color: $bg-color;
  border-radius: $border-radius-md;
}

.promise-icon {
  width: 40px;
  height: 40px;
  background-color: $bg-white;
  border-radius: $border-radius-md;
  display: flex;
  align-items: center;
  justify-content: center;
}

.promise-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.promise-title {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.promise-desc {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.contact-section {
  background-color: $bg-white;
  border-radius: $border-radius-lg;
  padding: $spacing-md;
  margin-bottom: $spacing-md;
  box-shadow: $shadow-sm;
}

.contact-list {
  display: flex;
  flex-direction: column;
  gap: $spacing-sm;
}

.contact-item {
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

.contact-icon {
  width: 40px;
  height: 40px;
  background-color: $bg-white;
  border-radius: $border-radius-md;
  display: flex;
  align-items: center;
  justify-content: center;
}

.contact-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: $spacing-xs;
}

.contact-label {
  font-size: $font-size-xs;
  color: $text-secondary;
}

.contact-value {
  font-size: $font-size-md;
  font-weight: 500;
  color: $text-primary;
}

.share-section {
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

  &.primary {
    background-color: $primary-color;
    color: $bg-white;
  }
}
</style>