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
    <todoList :tasks="tasks" />
  </div>
</template>

<style scoped>

</style>
