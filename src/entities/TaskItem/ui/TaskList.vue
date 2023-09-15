<template>
  <div class="list">
    <TaskComponent
      v-for="task in tasks"
      :key="task.id"
      :task="task"
      @onDone="$emit('onDone', task.id)"
      @onCancel="$emit('onCancel', task.id)"
    />
  </div>
</template>

<script setup lang="ts">
import type { TaskItem } from '../model/types/TaskItemTypes'
import TaskComponent from './TaskComponent.vue'
import { defineProps } from 'vue'

interface TaskListProps {
  tasks: TaskItem[]
}
defineProps<TaskListProps>()
</script>

<style lang="scss" scoped>
.list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
</style>
