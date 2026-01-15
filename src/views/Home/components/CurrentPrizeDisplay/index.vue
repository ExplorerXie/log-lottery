<script setup lang='ts'>
import { computed } from 'vue'
import { LotteryStatus } from '@/views/Home/type'

interface Props {
    currentStatus: LotteryStatus
    currentPrize: any
    isLotteryRunning: boolean
}

const props = defineProps<Props>()

// 计算是否显示当前奖品信息
const showPrizeInfo = computed(() => {
    return props.currentStatus === LotteryStatus.running || props.isLotteryRunning
})

// 计算奖品图片URL
const prizeImageUrl = computed(() => {
    if (props.currentPrize?.picture?.url) {
        return props.currentPrize.picture.url
    }
    return ''
})

// 计算奖品名称
const prizeName = computed(() => {
    if (props.currentPrize?.name) {
        return props.currentPrize.name
    }
    return '未选择奖品'
})
</script>

<template>
  <div v-if="showPrizeInfo && currentPrize" class="current-prize-display">
    <!-- 右上方：奖品照片放大显示（主要区域） -->
    <div v-if="prizeImageUrl" class="prize-image-container">
      <img :src="prizeImageUrl" :alt="prizeName" class="prize-image">
      <div class="prize-image-overlay">
        {{ prizeName }}
      </div>
    </div>
    
    <!-- 下方：简洁的文字提示 -->
    <div class="prize-info-container">
      <div class="current-drawing-text">
        当前正在抽取
      </div>
      <div class="prize-name-text">
        {{ prizeName }}
      </div>
      <div v-if="currentPrize.count && currentPrize.isUsedCount !== undefined" class="prize-count-text">
        剩余：{{ currentPrize.count - currentPrize.isUsedCount }}/{{ currentPrize.count }}
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.current-prize-display {
  position: fixed;
  top: 20px;
  right: 20px;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 12px;
  z-index: 1000;
  pointer-events: none;
  max-width: 300px;
  
  .prize-image-container {
    position: relative;
    width: 180px;
    height: 180px;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 12px 40px rgba(0, 0, 0, 0.4);
    border: 4px solid rgba(255, 215, 0, 0.9);
    background: linear-gradient(135deg, rgba(255, 215, 0, 0.3), rgba(255, 140, 0, 0.3));
    
    .prize-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    
    .prize-image-overlay {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0, 0, 0, 0.8);
      color: white;
      padding: 8px 10px;
      font-size: 14px;
      font-weight: bold;
      text-align: center;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
  }
  
  .prize-info-container {
    background: rgba(0, 0, 0, 0.8);
    backdrop-filter: blur(12px);
    border-radius: 12px;
    padding: 12px 18px;
    border: 2px solid rgba(255, 215, 0, 0.6);
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
    text-align: center;
    width: fit-content;
    
    .current-drawing-text {
      font-size: 16px;
      font-weight: 700;
      color: #ffd700;
      margin-bottom: 4px;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.6);
    }
    
    .prize-name-text {
      font-size: 24px;
      font-weight: 900;
      color: white;
      margin-bottom: 6px;
      text-shadow: 0 3px 6px rgba(0, 0, 0, 0.8);
      letter-spacing: 0.5px;
    }
    
    .prize-count-text {
      font-size: 14px;
      font-weight: 600;
      color: #a0a0ff;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.6);
    }
  }
}

// 响应式设计
@media (max-width: 768px) {
  .current-prize-display {
    top: 10px;
    right: 10px;
    gap: 8px;
    max-width: 200px;
    
    .prize-image-container {
      width: 120px;
      height: 120px;
      border-width: 3px;
    }
    
    .prize-info-container {
      padding: 10px 14px;
      
      .current-drawing-text {
        font-size: 14px;
      }
      
      .prize-name-text {
        font-size: 20px;
      }
      
      .prize-count-text {
        font-size: 12px;
      }
    }
  }
}

@media (max-width: 480px) {
  .current-prize-display {
    .prize-image-container {
      width: 100px;
      height: 100px;
    }
    
    .prize-info-container {
      padding: 8px 12px;
      
      .current-drawing-text {
        font-size: 12px;
      }
      
      .prize-name-text {
        font-size: 18px;
      }
    }
  }
}
</style>
