<template>
  <div class="scroll-tab-col" :style="{ width: clientWidth + 'px', height: clientHeight + 'px' }">
    <!-- // todo 空白页 -->
    <div v-if="!show">进入的时候是空白效果</div>
    <!-- // todo 具体的页面 -->
    <slot v-else></slot>
  </div>
</template>

<script setup>
import { clientWidth, clientHeight } from './clientParameters'
import { defineProps, watchEffect, ref, watch } from 'vue'
const props = defineProps({
  loading: Boolean
})
// todo 控制显示 空白页，还是挂载
const show = ref(false)
// todo 根据 props.loading 控制挂载
// const stop = watchEffect(() => {
//   if (props.loading) {
//     show.value = true
//     setTimeout(() => {
//       stop()
//     })
//   }
// })
const stop = watch(props, () => {
  if (props.loading) {
    show.value = true
    setTimeout(() => {
      stop()
    })
  }
},{
  immediate: true
})
</script>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>
