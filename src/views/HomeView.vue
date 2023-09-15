<script setup lang="ts">
import TaskList from '@/entities/TaskItem/ui/TaskList.vue'
import type { TaskItem } from '../entities/TaskItem/index'
import {computed, onMounted, ref} from 'vue'
import { useModal } from '@/hooks/useModal';
import AddTaskModal from '@/components/AddTaskModal/AddTaskModal.vue';
const tasks = ref<TaskItem[]>([
  {
    id: 1,
    text: 'First task',
    hasDone: false
  },
  {
    id: 2,
    text: 'TO do program',
    hasDone: false
  },
  {
    id: 3,
    text: 'Third task',
    hasDone: false
  },
  {
    id: 4,
    text: 'Clean house',
    hasDone: false
  }
])
const sortedList = computed(() => {
  return [...tasks.value].sort((a, b) => Number(a.hasDone) - Number(b.hasDone))
})
const doneTasks = ref<number[]>([])
const newTaskText = ref('')
const onDone = (id: number) => {
  const currentTask = tasks.value.find((task) => task.id === id)
  if (currentTask) {
    doneTasks.value.push(currentTask.id)
    currentTask.hasDone = true
    localStorage.setItem('doneTasks', JSON.stringify(doneTasks.value))
  }
}
const createTask = () => {
  tasks.value.push({
    id: tasks.value.length + 1,
    text: newTaskText.value,
    hasDone: false
  })
  newTaskText.value = ''
  closeModal()
}
const cancelTask = (id: number) => {
  const currentTask = tasks.value.find((task) => task.id === id)
  if (currentTask) {
    doneTasks.value = doneTasks.value.filter(taskId => taskId !== id)
    currentTask.hasDone = false
    localStorage.setItem('doneTasks', JSON.stringify(doneTasks.value))
  }
}
const [isVisible, {openModal, closeModal}] = useModal()
onMounted(() => {
  const storageTasks = localStorage.getItem('doneTasks')
  if (storageTasks) {
    doneTasks.value = JSON.parse(storageTasks)
    doneTasks.value.forEach(taskId => {
      const task = tasks.value.find(item => item.id === taskId)
      if (task) {
        task.hasDone = true
      }
    })
  }
})
</script>

<template>
  <main class="container">
    <h1 class="header">Tasks list</h1>
    <TaskList :tasks="sortedList" @onDone="onDone" @onCancel="cancelTask" />
    <button class="add_btn" @click="openModal">+</button>
  </main>
  <AddTaskModal :open="isVisible" :onClose="closeModal" @onCreate="createTask" v-model="newTaskText"/>
</template>

<style lang="scss">
.header {
  text-align: center;
}
.container {
  width: 1000px;
  margin: 0 auto;
}

.add_btn {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: #2EA950;
  border: none;
  color: #FFF;
  font-size: 32px;
  position: fixed;
  right: 30px;
  cursor: pointer;
  bottom: 30px;
}
</style>
