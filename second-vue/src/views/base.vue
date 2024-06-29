<!--
 * @Description: Vue组件模板和脚本示例
 * @Version: 1.0
 * @Autor: Li Cheng
 * @Date: 2024-06-24 15:18:28
 * @LastEditors: Li Cheng
 * @LastEditTime: 2024-06-24 15:22:19
-->
<template>
  <!-- 动态绑定class，年龄不同显示不同的class -->
  <h1 :class="'text-' + age" class="font-size-20">
    {{ name }} - {{ age === 16 ? '少年' : '成年人' }} - {{ sex(0) }}
  </h1>

  <!-- 使用v-bind动态绑定对象属性 -->
  <h1 v-bind="obj">{{ name }} - {{ age === 16 ? '少年' : '成年人' }} - {{ sex(0) }}</h1>

  <!-- 根据是否有弟弟显示不同内容 -->
  <h1 v-if="hasBro">有弟弟：王钢蛋</h1>
  <h2 v-if="hasBro">{{ state.name }} - {{ state.age }} - {{ state.sex }}</h2>
  <h1 v-else>没有弟弟</h1>

  <!-- 按钮点击事件，调用isBro和change方法 -->
  <button @click="isBro">疑问？</button>
  <button @click="change">切换</button>

  <!-- 动态绑定class，根据isActive状态切换 -->
  <h1 :class="[isActive ? 'active' : '']">李四</h1>
  <button @click="isActive = !isActive">选中李四</button>

  <!-- 动态绑定样式 -->
  <h1 :style="{ color: fontColor }">李四</h1>

  <!-- v-if, v-else-if和v-else条件渲染 -->
  <div v-if="type === 1">1</div>
  <div v-else-if="type === 2">2</div>
  <div v-else-if="type === 3">3</div>
  <div v-else>4</div>
</template>

<script setup>
import { ref, reactive } from 'vue';

// 定义响应式变量和函数
const name = ref('铁锤妹妹🔨');
const age = ref(18);

// 性别判断函数
function sex(sex) {
  return sex === 1 ? '男' : '女';
}

// 对象绑定属性
const obj = {
  class: 'text-16',
  id: 'font-size-24'
};

let hasBro = ref(false); // 是否有弟弟的标识
console.log(hasBro);

// 弹出提示并输出年龄到控制台
function isBro() {
  alert('到底有没有？');
  console.log(age);
}

// 定义响应式状态对象
const state = reactive({
  name: '王钢蛋',
  age: 16,
  sex: '男'
});

// 切换弟弟存在与否的状态，并修改状态属性
const change = () => {
  hasBro.value = !hasBro.value;
  console.log(hasBro);
  state.age = 17;
  console.log(state);
};

console.log(state);

// 选中状态
const isActive = ref(true);

// 字体颜色
const fontColor = ref('red');

// 类型判断变量
const type = ref(5);
</script>

<style scoped>
.text-16 {
  color: red;
}

.text-18 {
  color: blue;
}

.font-size-20 {
  font-size: 20px;
}

#font-size-24 {
  font-size: 24px;
}

.active {
  color: green;
}

.text-gray {
  color: gray;
}
</style>
