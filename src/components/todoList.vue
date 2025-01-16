<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps({
  tasks: {
    type: Array,
    required: true,
  },
});
</script>

<template>
  <el-table :data="tasks" style="width: 100%">
    <el-table-column prop="name" label="Name" width="180" />
    <el-table-column prop="category" label="Category" width="180" />
    <el-table-column prop="done" label="Done" width="180" />
  </el-table>
</template>

<style scoped>

</style>