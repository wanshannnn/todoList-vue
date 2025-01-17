<script lang="ts" setup>
import {reactive, ref} from 'vue'

const centerDialogVisible = ref(false)
const form = reactive({
  name: '',
  category: '',
  done: false,
  date: new Date(),
  description: '',
})

const emit = defineEmits(['addTask']);

function onSubmit() {
  // 校验
  if (!form.name || !form.category || !form.date) {
    console.error('Form is incomplete');
    alert('Please fill in all fields!');
    return;
  }
  // 将 Date 转换为 ISO 字符串
  const task = {
    name: form.name,
    category: form.category,
    done: form.done,
    date: form.date.toISOString(),
    description: form.description,
  };
  // 向父组件传递新任务
  emit('addTask', task);
  // 重置表单并关闭对话框
  form.name = '';
  form.category = '';
  form.date = new Date();
  form.description = '';
  centerDialogVisible.value = false;
}
</script>

<template>
  <el-button class="custom-button" @click="centerDialogVisible = true">Add Task</el-button>

  <el-dialog v-model="centerDialogVisible" title="Task Information" width="500" center>
    <el-form :model="form" label-width="auto" style="max-width: 600px">
      <el-form-item label="Name">
        <el-input v-model="form.name" placeholder="Please fill in the name"/>
      </el-form-item>
      <el-form-item label="Category">
        <el-select v-model="form.category" placeholder="please select the category">
          <el-option label="study" value="study" />
          <el-option label="work" value="work" />
          <el-option label="entertainment" value="entertainment" />
          <el-option label="other" value="other" />
        </el-select>
      </el-form-item>
      <el-form-item label="Deadline">
        <el-col :span="11">
          <el-date-picker
              v-model="form.date"
              type="date"
              placeholder="Pick a date"
              style="width: 100%"
          />
        </el-col>
      </el-form-item>
      <el-form-item label="Description">
        <el-input v-model="form.description" placeholder="Please fill in the description"/>
      </el-form-item>
    </el-form>
    <template #footer>
      <div class="dialog-footer">
        <el-button @click="centerDialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="onSubmit">Add</el-button>
      </div>
    </template>
  </el-dialog>

</template>

<style scoped>
.custom-button {
  background-color: pink;
  border-radius: 12px;
  color: white;
  border: none;
  transition: background-color 0.3s ease;
}

.custom-button:hover {
  background-color: #ffa3c9;
}
</style>