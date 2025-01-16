<script lang="ts" setup>
import {reactive, ref} from 'vue'

const centerDialogVisible = ref(false)
const form = reactive({
  name: '',
  category: '',
  done: false,
})

const emit = defineEmits(['addTask']);

function onSubmit() {
  // 校验
  if (!form.name || !form.category) {
    console.error('Form is incomplete');
    alert('Please fill in all fields!');
    return;
  }
  // 向父组件传递新任务
  emit('addTask', { ...form });
  // 重置表单并关闭对话框
  form.name = '';
  form.category = '';
  centerDialogVisible.value = false;
}
</script>

<template>
  <el-button plain @click="centerDialogVisible = true">Add Task</el-button>

  <el-dialog v-model="centerDialogVisible" title="Task Information" width="500" center>
    <el-form :model="form" label-width="auto" style="max-width: 600px">
      <el-form-item label="Name">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="Category">
        <el-select v-model="form.category" placeholder="please select the category">
          <el-option label="study" value="study" />
          <el-option label="work" value="work" />
          <el-option label="entertainment" value="entertainment" />
          <el-option label="other" value="other" />
        </el-select>
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

</style>