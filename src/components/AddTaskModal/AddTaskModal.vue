<template>
  <AppModal :open="open" :on-close="onClose">
    <div class="task-modal">
      <h3 class="header">Create new task</h3>
      <input class="input" type="text" @change="updateInput" :value="modelValue" />
      <button @click="createTask" class="create_btn">Create</button>
    </div>
  </AppModal>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AppModal from '../Modal/AppModal.vue'

interface AddTaskModalProps {
  open?: boolean
  onClose?: () => void
  modelValue: string
}
defineProps<AddTaskModalProps>()
const emit = defineEmits(['update:modelValue', 'onCreate'])

const createTask = () => {
  emit('onCreate')
}

const updateInput = (event: Event) => {
  emit('update:modelValue', (event.target as HTMLInputElement).value)
}
</script>

<style lang="scss" scoped>
.header {
  color: #121213;
  text-align: center;
  font-size: 20px;
  font-weight: 600;
}
.create_btn {
  width: 150px;
  height: 40px;
  font-size: 16px;
  border: 0;
  outline: none;
  border-radius: 10px;
  color: #FFFFFF;
  background: #6587FF;
  cursor: pointer;
}

.task-modal {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
}

.input {
  width: 100%;
  height: 30px;
  border-radius: 10px;
  border: 1px solid #E7E7E7;
  outline: none;
  padding: 20px 30px;
  background-color: #fff;
}
</style>
