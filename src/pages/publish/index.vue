<script setup lang="ts">
import { ref, computed } from 'vue'
import { onLoad } from '@dcloudio/uni-app'

// 服务类型
const serviceType = ref('')

// 表单数据
const formData = ref({
  itemType: '',
  itemDesc: '',
  pickupAddress: '',
  deliveryAddress: '',
  pickupTime: '',
  deliveryTime: '',
  specialRequirements: '',
  contactPhone: '',
  contactName: ''
})

// 价格计算
const price = computed(() => {
  // 基础价格
  let basePrice = 10

  // 根据服务类型调整价格
  if (serviceType.value === '打印材料') {
    basePrice = 8
  } else if (serviceType.value === '取外卖') {
    basePrice = 12
  } else if (serviceType.value === '搬东西') {
    basePrice = 20
  } else if (serviceType.value === '代购物') {
    basePrice = 15
  } else if (serviceType.value === '送文件') {
    basePrice = 10
  }

  return basePrice
})

// 页面加载
onLoad((options) => {
  if (options?.type) {
    serviceType.value = decodeURIComponent(options.type)
    formData.value.itemType = serviceType.value
  }
})

// 提交表单
const handleSubmit = () => {
  // 验证表单
  if (!formData.value.itemDesc) {
    uni.showToast({
      title: '请填写物品描述',
      icon: 'none'
    })
    return
  }

  if (!formData.value.pickupAddress) {
    uni.showToast({
      title: '请填写取货地址',
      icon: 'none'
    })
    return
  }

  if (!formData.value.deliveryAddress) {
    uni.showToast({
      title: '请填写送达地址',
      icon: 'none'
    })
    return
  }

  // 跳转到价格页面
  uni.navigateTo({
    url: `/pages/price/index?data=${encodeURIComponent(JSON.stringify({
      ...formData.value,
      price: price.value
    }))}`
  })
}

// 重置表单
const handleReset = () => {
  formData.value = {
    itemType: serviceType.value,
    itemDesc: '',
    pickupAddress: '',
    deliveryAddress: '',
    pickupTime: '',
    deliveryTime: '',
    specialRequirements: '',
    contactPhone: '',
    contactName: ''
  }
}
</script>

<template>
  <view class="container">
    <view class="form-section">
      <view class="section-title">
        <text class="title-text">基本信息</text>
      </view>

      <view class="form-item">
        <text class="form-label">服务类型</text>
        <input
          v-model="formData.itemType"
          class="form-input"
          placeholder="请选择服务类型"
          disabled
        />
      </view>

      <view class="form-item">
        <text class="form-label">物品描述</text>
        <textarea
          v-model="formData.itemDesc"
          class="form-textarea"
          placeholder="请详细描述需要跑腿的物品"
          :maxlength="200"
        />
      </view>
    </view>

    <view class="form-section">
      <view class="section-title">
        <text class="title-text">地址信息</text>
      </view>

      <view class="form-item">
        <text class="form-label">取货地址</text>
        <input
          v-model="formData.pickupAddress"
          class="form-input"
          placeholder="请输入取货地址"
        />
      </view>

      <view class="form-item">
        <text class="form-label">送达地址</text>
        <input
          v-model="formData.deliveryAddress"
          class="form-input"
          placeholder="请输入送达地址"
        />
      </view>
    </view>

    <view class="form-section">
      <view class="section-title">
        <text class="title-text">时间要求</text>
      </view>

      <view class="form-item">
        <text class="form-label">取货时间</text>
        <picker
          mode="time"
          :value="formData.pickupTime"
          @change="formData.pickupTime = $event.detail.value"
        >
          <view class="form-input picker-input">
            {{ formData.pickupTime || '请选择取货时间' }}
          </view>
        </picker>
      </view>

      <view class="form-item">
        <text class="form-label">送达时间</text>
        <picker
          mode="time"
          :value="formData.deliveryTime"
          @change="formData.deliveryTime = $event.detail.value"
        >
          <view class="form-input picker-input">
            {{ formData.deliveryTime || '请选择送达时间' }}
          </view>
        </picker>
      </view>
    </view>

    <view class="form-section">
      <view class="section-title">
        <text class="title-text">联系方式</text>
      </view>

      <view class="form-item">
        <text class="form-label">联系人</text>
        <input
          v-model="formData.contactName"
          class="form-input"
          placeholder="请输入联系人姓名"
        />
      </view>

      <view class="form-item">
        <text class="form-label">联系电话</text>
        <input
          v-model="formData.contactPhone"
          class="form-input"
          placeholder="请输入联系电话"
          type="number"
        />
      </view>
    </view>

    <view class="form-section">
      <view class="section-title">
        <text class="title-text">特殊要求</text>
      </view>

      <view class="form-item">
        <textarea
          v-model="formData.specialRequirements"
          class="form-textarea"
          placeholder="如有特殊要求请填写"
          :maxlength="100"
        />
      </view>
    </view>

    <view class="price-preview">
      <text class="price-label">预估价格</text>
      <text class="price-value">¥{{ price }}</text>
    </view>

    <view class="button-group">
      <button class="btn btn-secondary" @click="handleReset">
        重置
      </button>
      <button class="btn btn-primary" @click="handleSubmit">
        下一步
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

.form-section {
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

.form-item {
  margin-bottom: $spacing-md;
}

.form-label {
  display: block;
  font-size: $font-size-sm;
  color: $text-secondary;
  margin-bottom: $spacing-xs;
}

.form-input {
  width: 100%;
  padding: $spacing-sm $spacing-md;
  border: 1px solid $border-color;
  border-radius: $border-radius-md;
  font-size: $font-size-md;
  background-color: $bg-white;

  &:disabled {
    background-color: $bg-color;
    color: $text-tertiary;
  }
}

.picker-input {
  display: flex;
  align-items: center;
  color: $text-primary;

  &:empty::before {
    content: attr(placeholder);
    color: $text-tertiary;
  }
}

.form-textarea {
  width: 100%;
  padding: $spacing-sm $spacing-md;
  border: 1px solid $border-color;
  border-radius: $border-radius-md;
  font-size: $font-size-md;
  background-color: $bg-white;
  min-height: 80px;
}

.price-preview {
  background: linear-gradient(135deg, $primary-color 0%, $primary-light 100%);
  border-radius: $border-radius-lg;
  padding: $spacing-lg;
  margin-bottom: $spacing-md;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: $bg-white;
  box-shadow: $shadow-md;
}

.price-label {
  font-size: $font-size-md;
}

.price-value {
  font-size: $font-size-xxl;
  font-weight: bold;
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