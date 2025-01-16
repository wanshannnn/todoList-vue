<script setup lang="ts">
import {computed, defineProps, ref} from 'vue';

const props = defineProps({
  tasks: {
    type: Array as () => Array<{ name: string; category: string; done: boolean }>,
    required: true,
  },
});

const completedTasks = computed(() => props.tasks.filter(task => task.done));
const incompleteTasks = computed(() => props.tasks.filter(task => !task.done));

function toggleTaskStatus(task: { name: string; category: string; done: boolean }) {
  task.done = !task.done;
}
</script>

<template>
  <div class="todoList-container">
    <div class="incomplete-container">
      <h3>To Do</h3>
      <el-table :data="incompleteTasks" style="width: 100%">
        <el-table-column prop="name" label="Name" width="200" />
        <el-table-column prop="category" label="Category" width="180" />
        <el-table-column label="Status" width="180">
          <template #default="{ row }">
            <el-checkbox :checked="!row.done" @change="toggleTaskStatus(row)">
              Undone
            </el-checkbox>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <div class="completed-container">
      <h3>Done</h3>
      <el-table :data="completedTasks" style="width: 100%">
        <el-table-column prop="name" label="Name" width="200" />
        <el-table-column prop="category" label="Category" width="180" />
        <el-table-column label="Status" width="180">
          <template #default="{ row }">
            <el-checkbox :checked="row.done" @change="toggleTaskStatus(row)">
              Done
            </el-checkbox>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<style scoped>
.todoList-container{
  width: 100%;
  margin: 20px auto;
  box-sizing: border-box;
}

</style>