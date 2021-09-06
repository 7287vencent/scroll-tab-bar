<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  msg: String
})
onMounted(() => {
  console.log("挂载第", props.msg, "个页面")
})
</script>

<template>
  <h1>{{ msg }}</h1>
</template>

<style scoped>
a {
  color: #42b983;
}
</style>
