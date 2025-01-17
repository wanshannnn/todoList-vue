<script setup lang="ts">
import {computed, defineProps, ref} from 'vue';

// 接收父组件传递的 tasks 信息
const props = defineProps({
  tasks: {
    type: Array as () => Array<{ id: number; name: string; category: string; done: boolean; date: Date; description?: string }>,
    required: true,
  },
});

// 根据 done 属性分类出已完成和未完成的 tasks
const completedTasks = computed(() => props.tasks.filter(task => task.done));
const incompleteTasks = computed(() => props.tasks.filter(task => !task.done));

const currentTask = ref<{ id: number; name: string; category: string; done: boolean; date: Date; description?: string }>({
  id: 0,
  name: '',
  category: '',
  done: false,
  date: new Date(),
  description: '',
});
const isDialogVisible = ref(false);
const emit = defineEmits(['updateTask']);

function formatDate(date: Date): string {
  const options: Intl.DateTimeFormatOptions = { month: 'short', day: 'numeric' };
  return new Intl.DateTimeFormat('en-US', options).format(new Date(date));
}

function toggleTaskStatus(id: number, newStatus: boolean) {
  const updatedTasks = props.tasks.map(task =>
      task.id === id ? { ...task, done: newStatus } : task
  );
  emit('updateTask', updatedTasks);
}

function showDetail(task: { id: number; name: string; category: string; done: boolean; date: Date; description?: string }) {
  currentTask.value = { ...task };
  isDialogVisible.value = true;
}

function updateTask() {
  // 如果当前任务的 id 等于正在编辑的任务 currentTask.value 的 id，那么使用正在编辑的任务替换该任务，实现更新。
  const updatedTasks = props.tasks.map(task =>
      task.id === currentTask.value.id ? { ...currentTask.value } : task
  );
  emit('updateTask', updatedTasks);
  isDialogVisible.value = false;
}

function deleteTask() {
  // 保留所有 id 不等于 currentTask.value.id 的任务。将匹配到的任务移除。
  const updatedTasks = props.tasks.filter(task => task.id !== currentTask.value.id);
  emit('updateTask', updatedTasks);
  isDialogVisible.value = false;
}

function clearCompleted() {
  const updatedTasks = props.tasks.filter((task) => !task.done);
  emit("updateTask", updatedTasks);
}
</script>

<template>
  <div class="todoList-container">
    <!-- 未完成的任务 -->
    <div class="incomplete-container">
      <h3>To Do</h3>
      <el-table :data="incompleteTasks" style="width: 100%">
        <el-table-column prop="name" label="Name" width="120" />
        <el-table-column prop="category" label="Category" width="120" />
        <el-table-column prop="date" label="Deadline" width="100" :formatter="(row: any) => formatDate(row.date)"/>
        <el-table-column label="Status" width="120" >
          <template #default="{ row }">
            <el-checkbox :checked="row.done" @change="(checked: boolean) => toggleTaskStatus(row.id, checked)">
              {{ row.done ? 'Done' : 'Undone' }}
            </el-checkbox>
          </template>
        </el-table-column>
        <el-table-column label="Detail" width="100">
          <template #default="{ row }">
            <el-button @click="showDetail(row)" type="text" class="custom-button-text">View</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 已完成的任务 -->
    <div class="completed-container">
      <div class="completed-header">
        <h3>Done</h3>
        <el-button type="danger" size="mini" link @click="clearCompleted">Clear</el-button>
      </div>
      <el-table :data="completedTasks" style="width: 100%">
        <el-table-column prop="name" label="Name" width="340" />
        <el-table-column label="Status" width="120">
          <template #default="{ row }">
            <el-checkbox :checked="row.done" @change="(checked: boolean) => toggleTaskStatus(row.id, checked)">
              {{ row.done ? 'Done' : 'Undone' }}
            </el-checkbox>
          </template>
        </el-table-column>
        <el-table-column label="Detail" width="100">
          <template #default="{ row }">
            <el-button @click="showDetail(row)" type="text" class="custom-button-text">View</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <!-- 弹窗展示任务详情 -->
    <el-dialog v-model="isDialogVisible" title="Task Detail" width="500px" center>
      <el-form :model="currentTask" label-width="auto">
        <el-form-item label="Name">
          <el-input v-model="currentTask.name" />
        </el-form-item>
        <el-form-item label="Category">
          <el-select v-model="currentTask.category">
            <el-option label="study" value="study" />
            <el-option label="work" value="work" />
            <el-option label="entertainment" value="entertainment" />
            <el-option label="other" value="other" />
          </el-select>
        </el-form-item>
        <el-form-item label="Description">
          <el-input v-model="currentTask.description" type="textarea" />
        </el-form-item>
        <el-form-item label="Date">
          <el-date-picker v-model="currentTask.date" type="date" />
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="isDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="updateTask">Save</el-button>
        <el-button type="danger" @click="deleteTask">Delete</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
.todoList-container{
  width: 100%;
  margin: 20px auto;
  margin-top: 0px;
  box-sizing: border-box;
}

.completed-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

:deep(.el-checkbox__input.is-checked .el-checkbox__inner) {
  background-color: pink !important;
  border-color: pink !important;
}

:deep(.el-checkbox__input .el-checkbox__inner:hover) {
  border-color: #ffa3c9 !important;
}

:deep(.el-checkbox__input .el-checkbox__inner) {
  border-color: pink !important;
}

:deep(.el-checkbox__label) {
  color: pink !important;
}

.custom-button-text {
  color: pink !important;
}

.custom-button-text:hover {
  color: #ffa3c9 !important;
}

h3 {
  color: #333;
  font-family: 'Georgia', serif;
  font-size: 20px;
  font-weight: bold;
}
</style>