<script setup lang="ts">
import todoList from "@/components/todoList.vue";
import todoInput from "./components/todoInput.vue";
import {ref} from "vue";

const tasks = ref([
  { id: 1, name: 'Task 1', category: 'work', done: false, date:new Date() },
  { id: 2, name: 'Task 2', category: 'study', done: false, date:new Date() },
]);

let nextId = 3;

function addTask(task: { name: string; category: string; done: boolean; date: string; description: string }) {
  tasks.value.push({
    id: nextId++,
    ...task,
    // 将 ISO 字符串转换回 Date 对象
    date: new Date(task.date),
  });
}

function updateTask(updatedTasks: Array<{ id: number; name: string; category: string; done: boolean; date: Date; description?: string }>) {
  tasks.value = updatedTasks;
}
</script>

<template>
  <div class="app-container">
    <p class="todo-title">To Do List</p>
    <todoInput @addTask="addTask" />
    <el-divider />
    <todoList :tasks="tasks" @updateTask="updateTask"/>
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

.todo-title {
  color: pink;
  font-family: 'Georgia', serif;
  font-size: 24px;
  font-weight: bold;
}
</style>
