<template>
  <div :class="['task', { crossed: task.hasDone }]">
    <p>{{ task.id }}</p>
    <p class="text">{{ task.text }}</p>
    <div class="buttons">
      <button @click="$emit('onCancel')" :class="['btn cancel', {inactive: !task.hasDone}]">Not done</button>
      <button @click="$emit('onDone')" class="btn done">Done</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { TaskItem } from '../model/types/TaskItemTypes'
import { defineProps } from 'vue'
interface TaskItemProps {
  task: TaskItem
}

defineProps<TaskItemProps>()
</script>

<style lang="scss" scoped>
.task {
  width: 1000px;
  font-size: 16px;
  height: 50px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 10px;
  padding: 0 15px;
  background: #e7e7e7;
}

.btn {
  border: 0;
  outline: none;
  padding: 10px;
  font-size: 16px;
  background: #2EA950;
  border-radius: 10px;
  color: #FFF;
  cursor: pointer;
}

.cancel {
  background: #F55454;
}

.buttons {
  display: flex;
  gap: 10px;
}

.inactive {
  pointer-events: none;
  opacity: 0.5;
}

.crossed {
  .text {
    text-decoration: line-through;

  }
  .done {
    opacity: 0.5;
    pointer-events: none;
  }
}
</style>
