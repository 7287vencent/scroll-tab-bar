<script setup>

import { ref } from 'vue'
import ScrollTab from './components/ScrollTab.vue'
import ScrollTabCol from './components/ScrollTabCol.vue'
import Page from './components/Page.vue'
let tabIndex = ref(0) // todo 控制显示哪个页面
let loadIndex = ref(0) // todo 控制加载哪个页面
// todo 通知页面切换
const selectChange = (value) => {
  tabIndex.value = value
  loadIndex.value = value
}
</script>

<template>
  <!-- // todo 顶部菜单 -->
  <ul class="label-list">
    <li
      class="label-list-item"
      :class="{ 'select-label-list-item': tabIndex === 0 }"
      @click="tabIndex = 0"
    >1</li>
    <li
      class="label-list-item"
      :class="{ 'select-label-list-item': tabIndex === 1 }"
      @click="tabIndex = 1"
    >2</li>
    <li
      class="label-list-item"
      :class="{ 'select-label-list-item': tabIndex === 2 }"
      @click="tabIndex = 2"
    >3</li>
    <li
      class="label-list-item"
      :class="{ 'select-label-list-item': tabIndex === 3 }"
      @click="tabIndex = 3"
    >4</li>
    <li
      class="label-list-item"
      :class="{ 'select-label-list-item': tabIndex === 4 }"
      @click="tabIndex = 4"
    >5</li>
  </ul>
  <!-- // todo 页面 -->
  <ScrollTab :tabIndex="tabIndex" @selectChange="selectChange">
    <ScrollTabCol class="item" :loading="loadIndex === 0">
      <Page msg="1"></Page>
    </ScrollTabCol>
    <ScrollTabCol class="item" :loading="loadIndex === 1">
      <Page msg="2"></Page>
    </ScrollTabCol>
    <ScrollTabCol class="item" :loading="loadIndex === 2">
      <Page msg="3"></Page>
    </ScrollTabCol>
    <ScrollTabCol class="item" :loading="loadIndex === 3">
      <Page msg="4"></Page>
    </ScrollTabCol>
    <ScrollTabCol class="item" :loading="loadIndex === 4">
      <Page msg="5"></Page>
    </ScrollTabCol>
  </ScrollTab>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
html,
body,
div,
body,
ul,
li {
  margin: 0;
  padding: 0;
  list-style: none;
  text-decoration: none;
}
.label-list {
  position: fixed;
  width: 100%;
  z-index: 100;
  display: flex;
  height: 50px;
  line-height: 50px;
}

.label-list-item {
  flex-grow: 1;
  background: #909399;
  color: #fff;
  text-align: center;
}

.select-label-list-item {
  font-weight: 600;
  background: #67c23a;
}

.item {
  text-align: center;
  background: #409eff;
  line-height: 400px;
  color: #fff;
  font-size: 100px;
  font-weight: 600;
}
</style>
