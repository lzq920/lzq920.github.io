---
title: 虚拟列表的基础实现
date: 2022-01-06 11:11
---

虚拟列表是前端长列表中的一种解决方案，使用 Vue3.x 实现一个基础的虚拟列表

```
<script setup>
import { reactive, ref, computed, onMounted } from 'vue';
//初始化10万条数据
const list = reactive(
  Array(10000000)
    .fill(0)
    .map((item, index) => `item${index}`)
);
//监听滚动事件
const handleScroll = (e) => {
  window.requestAnimationFrame(() => updateVisibleData(e.target.scrollTop));
};
//要显示的数据
const visibleList = ref([]);
//每一项的高度
const itemHeight = 30;
//总体内容的高度
const contentHeight = computed(() => list.length * itemHeight + 'px');
const rootRef = ref(null);
const contentRef = ref(null);
//更新显示数据
const updateVisibleData = (scrollTop = 0) => {
  //计算容器可以显示的数量
  const visibleCount = Math.ceil(rootRef.value.clientHeight / itemHeight);
  //计算当前已经超出显示范围的下标
  const start = Math.round(scrollTop / itemHeight);
  //计算当前要显示的数据下标
  const end = start + visibleCount;
  visibleList.value = list.slice(start, end);
  //计算偏移量
  contentRef.value.style.webkitTransform = `translate3d(0, ${
    start * itemHeight
  }px, 0)`;
};
onMounted(() => {
  updateVisibleData();
});
</script>

<template>
  <div class="list-view" @scroll="handleScroll" ref="rootRef">
    <div class="list-view-phantom" :style="{ height: contentHeight }"></div>
    <div class="list-view-content" ref="contentRef">
      <div
        class="list-view-item"
        v-for="(item, index) in visibleList"
        :key="item"
      >
        {{ item }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.list-view {
  height: 400px;
  overflow: auto;
  position: relative;
  border: 1px solid #aaa;
}

.list-view-phantom {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}

.list-view-content {
  left: 0;
  right: 0;
  top: 0;
  position: absolute;
}

.list-view-item {
  padding: 5px;
  color: #666;
  line-height: 30px;
  box-sizing: border-box;
}
</style>

```
