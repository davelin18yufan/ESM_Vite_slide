<template>
  <div class="relative" :style="{ width: size + 'px', height: size + 'px' }">
    <svg
      class="transform -rotate-90"
      :width="size"
      :height="size"
    >
      <!-- 背景圓圈 -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius"
        fill="none"
        stroke="#E5E7EB"
        :stroke-width="strokeWidth"
      />
      <!-- 進度圓圈 -->
      <circle
        :cx="center"
        :cy="center"
        :r="radius"
        fill="none"
        stroke="rgb(164, 251, 98)"
        :stroke-width="strokeWidth"
        :stroke-dasharray="circumference"
        :stroke-dashoffset="strokeDashoffset"
        stroke-linecap="round"
        class="transition-all duration-500 ease-in-out"
      />
    </svg>
    <!-- 中間的數字 -->
    <div 
      class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 font-bold text-lime-200"
      :style="{ fontSize: size / 5 + 'px' }"
    >
      {{ validPercentage }}%
    </div>
  </div>
</template>

<script>
export default {
  name: 'CirclePercentage',
  props: {
    percentage: {
      type: Number,
      default: 75
    },
    size: {
      type: Number,
      default: 160
    },
    strokeWidth: {
      type: Number,
      default: 10
    },
    strokeColor: {
      type: String,
      default: 'rgb(164, 251, 98)'
    }
  },
  computed: {
    // 確保百分比在 0-100 之間
    validPercentage() {
      return Math.min(Math.max(this.percentage, 0), 100)
    },
    // 計算圓心位置
    center() {
      return this.size / 2
    },
    // 計算半徑
    radius() {
      return (this.size - this.strokeWidth) / 2
    },
    // 計算圓的周長
    circumference() {
      return 2 * Math.PI * this.radius
    },
    // 計算進度條偏移量
    strokeDashoffset() {
      return this.circumference - (this.validPercentage / 100) * this.circumference
    }
  }
}
</script>

<style scoped>
.transition-all {
  transition: all 0.5s ease-in-out;
}
</style>