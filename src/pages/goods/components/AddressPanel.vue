<script setup lang="ts">
import { useAddressStore } from '@/stores/modules/address'
import { ref } from 'vue'
import type { AddressItem } from '@/types/address'
import { getMemberAddressAPI } from '@/services/address'
// 子调父
const emit = defineEmits<{
  (event: 'close'): void
  (event: 'changeSelectedAddress', item: AddressItem): void
}>()
const addressStore = defineProps<{
  selectedAddress?: AddressItem
  addressList: AddressItem[]
}>()
console.log(addressStore.selectedAddress)

// // const addressList = ref<AddressItem[]>()
// // // 获取用户地址列表数据
// // const getMemberAddressData = async () => {
// //   const res = await getMemberAddressAPI()
// //   addressList.value = res.result
// // }
// getMemberAddressData()
const isFirst = ref(true)
// 当点击选择地址
const onSelectAddress = (item: AddressItem) => {
  // 将选中地址存入父组件pinia地址仓库
  // addressStore.changeSelectedAddress(item)
  emit('changeSelectedAddress', item)
  isFirst.value = false
  emit('close')
}

// 新建地址
const toCreateAddress = () => {
  uni.navigateTo({ url: '/pagesMember/address-form/address-form' })
}
</script>

<template>
  <view class="address-panel">
    <!-- 关闭按钮 -->
    <text class="close icon-close" @tap="emit('close')"></text>
    <!-- 标题 -->
    <view class="title">配送至</view>
    <!-- 内容 -->
    <view class="content">
      <view
        class="item"
        @tap="onSelectAddress(item)"
        v-for="item in addressStore.addressList"
        :key="item.id"
      >
        <view class="user">{{ item.receiver }} {{ item.contact }}</view>
        <view class="address">{{ item.fullLocation }} {{ item.address }}</view>
        <text
          class="icon"
          :class="{
            'icon-checked': selectedAddress?.id === item.id || (item.isDefault && isFirst),
            'icon-ring': !(selectedAddress?.id === item.id || (item.isDefault && isFirst)),
          }"
        ></text>
      </view>
    </view>
    <view class="footer">
      <view class="button primary" @tap="toCreateAddress"> 新建地址 </view>
      <view v-if="false" class="button primary">确定</view>
    </view>
  </view>
</template>

<style lang="scss">
.address-panel {
  padding: 0 30rpx;
  border-radius: 10rpx 10rpx 0 0;
  position: relative;
  background-color: #fff;
}

.title {
  line-height: 1;
  padding: 40rpx 0;
  text-align: center;
  font-size: 32rpx;
  font-weight: normal;
  border-bottom: 1rpx solid #ddd;
  color: #444;
}

.close {
  position: absolute;
  right: 24rpx;
  top: 24rpx;
}

.content {
  min-height: 300rpx;
  max-height: 540rpx;
  overflow: auto;
  padding: 20rpx;

  .item {
    padding: 30rpx 50rpx 30rpx 60rpx;
    background-size: 40rpx;
    background-repeat: no-repeat;
    background-position: 0 center;
    background-image: url(https://pcapi-xiaotuxian-front-devtest.itheima.net/miniapp/images/locate.png);
    position: relative;
  }

  .icon {
    color: #999;
    font-size: 40rpx;
    transform: translateY(-50%);
    position: absolute;
    top: 50%;
    right: 0;
  }

  .icon-checked {
    color: #27ba9b;
  }

  .icon-ring {
    color: #444;
  }

  .user {
    font-size: 28rpx;
    color: #444;
    font-weight: 500;
  }

  .address {
    font-size: 26rpx;
    color: #666;
  }
}

.footer {
  display: flex;
  justify-content: space-between;
  padding: 20rpx 0 40rpx;
  font-size: 28rpx;
  color: #444;

  .button {
    flex: 1;
    height: 72rpx;
    text-align: center;
    line-height: 72rpx;
    margin: 0 20rpx;
    color: #fff;
    border-radius: 72rpx;
  }

  .primary {
    color: #fff;
    background-color: #27ba9b;
  }

  .secondary {
    background-color: #ffa868;
  }
}
</style>
