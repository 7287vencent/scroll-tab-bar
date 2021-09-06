<template>
  <div class="scroll-tab">
    <div
      class="scroll-tab-item"
      :style="{ width: scrollWidth + 'px', transform: `translate(${contentBoxLeft}px, 0)`, transitionDuration: delay + 's' }"
      ref="scrollTabItem"
      @touchstart="handleTouchStart($event)"
      @touchmove="handleTouchMove($event)"
      @touchend="handleTouchEnd($event)"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { clientWidth, clientHeight } from './clientParameters'
import { ref, watch, onMounted, reactive, defineProps, toRefs, toRef } from 'vue'
const props = defineProps({
  tabIndex: Number,
})
const emit = defineEmits(['selectChange'])
watch(props, () => {
  contentBoxLeft.value = calcluateBoxLeft(props.tabIndex)
  tabIndex.value = props.tabIndex
})

const scrollWidth = ref(0)
const contentBoxLeft = ref(0)
const delay = ref(0.3)
const scrollTabItem = ref(null)

let tabIndex = usetabIndex(emit)
let children = reactive([])
let length = ref(0)

const { calcluateBoxLeft, handleTouchStart, handleTouchMove, handleTouchEnd } = useMoveTouch(tabIndex, contentBoxLeft, clientWidth, length, scrollWidth)

onMounted(() => {
  calculateScrollWidth()
})
// ? 计算 scrollWidth
const calculateScrollWidth = () => {
  children = scrollTabItem.value.children
  length.value = children.length
  scrollWidth.value = children.length * clientWidth
}


</script>


<script>
const usetabIndex = (emit) => {
  let tabIndex = ref(0)
  watch(tabIndex, () => {
    emit('selectChange', tabIndex.value)
  })
  return tabIndex
}
/**
 * @param tabIndex 当前显示的 page
 * @param contentBoxLeft 当前轮播的位置
 * @param clientWidth 页面的宽度
 * @param length 页面的数量
 * @param scrollWidth 轮播图总的宽度
 */
const useMoveTouch = (tabIndex, contentBoxLeft, clientWidth, length, scrollWidth) => {
  let moveOptions = reactive({
    direction: '', // ? 方向
    isStart: false, // ? 是否按下
    startPos: null, // ? 记录开始的位置
    endPos: null // ? 记录结束的位置
  })
  const calcluateBoxLeft = (index) => {
    window.scrollTo(0, 0)
    return -(clientWidth * index)
  }

  const handleTouchStart = (e) => {
    moveOptions.isStart = true
    moveOptions.startPos = e.touches[0]
    moveOptions.endPos = e.touches[0]
  }
  const handleTouchMove = (e) => {
    // ? 根据当前 e的位置，判断出是向 上下 还是 左右滑动
    let left = e.touches[0].clientX - moveOptions.endPos.clientX
    let top = e.touches[0].clientY - moveOptions.endPos.clientY
    if (moveOptions.isStart) {
      moveOptions.direction = (Math.abs(left) > Math.abs(top)) ? 'X' : 'Y'
    }
    moveOptions.endPos = e.touches[0]
    if (moveOptions.direction === 'X') {
      e.preventDefault()
      // todo 左右移动
      // ? 1. 定义移动的 速率
      let coefficient = 1
      if (contentBoxLeft.value >= 20 || contentBoxLeft.value < -(scrollWidth.value - clientWidth + 20)) {
        coefficient = 0
      } else if (left >= 0 && tabIndex.value === 0) {
        coefficient = 0.3
      } else if (left <= 0 && tabIndex.value === length.value - 1) {
        coefficient = 0.3
      }
      contentBoxLeft.value += coefficient * left
    }
  }
  const handleTouchEnd = (e) => {
    if (moveOptions.direction !== 'X') return
    // ? 1. 获取移动的距离
    const moveLen = moveOptions.endPos.clientX - moveOptions.startPos.clientX
    if (moveLen > 80 && tabIndex.value !== 0) {
      tabIndex.value--
    } else if (moveLen < -80 && tabIndex.value !== length.value - 1) {
      tabIndex.value++
    }
    // ? 计算出偏移量
    contentBoxLeft.value = calcluateBoxLeft(tabIndex.value)
  }
  return {
    calcluateBoxLeft,
    handleTouchStart,
    handleTouchMove,
    handleTouchEnd
  }
}

</script>


<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.scroll-tab {
  width: 100%;
  overflow: hidden;
}
.scroll-tab-item {
  display: flex;
  flex-wrap: nowrap;
  align-items: flex-start;
}
</style>
