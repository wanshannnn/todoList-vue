<script setup lang="ts">
import todoList from "@/components/todoList.vue";
import todoInput from "./components/todoInput.vue";
import {ref} from "vue";

const tasks = ref([
  { name: 'Example Task 1', category: 'work', done: false },
  { name: 'Example Task 2', category: 'study', done: false },
]);

function addTask(task: { name: string; category: string; done: boolean }) {
  tasks.value.push(task);
}
</script>

<template>
  <div class="app-container">
    <p>To Do List</p>
    <todoInput @addTask="addTask" />
    <el-divider />
    <todoList :tasks="tasks" />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  flex-direction: column; /* 垂直排列 */
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.5); /* 半透明白色 */
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  width: 100%;
  margin: 20px auto;
  padding: 20px;
  box-sizing: border-box;
}

p {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-bottom: 16px;
  text-align: center;
}
</style>
