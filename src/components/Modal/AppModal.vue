<script setup lang="ts">
import CloseIcon from '../../assets/close.svg'
import { onMounted, onUnmounted } from 'vue'
import ModalWrapper from './ModalWrapper.vue'

interface ModalProps {
  open?: boolean
  onClose?: () => void
}

const props = defineProps<ModalProps>()

const onKeyDown = (event: KeyboardEvent) => {
  if (event.key === 'Escape' && props.open) {
    props.onClose?.()
  }
}

onMounted(() => {
  document.body.addEventListener('keydown', onKeyDown)
})

onUnmounted(() => {
  document.body.removeEventListener('keydown', onKeyDown)
})
</script>

<template>
  <ModalWrapper :open="open" :on-close="onClose">
    <div class="content">
      <slot />
      <CloseIcon @click="onClose" class="close" />
    </div>
  </ModalWrapper>
</template>

<style scoped lang="scss">
.content {
  position: relative;
  max-width: 60%;
  background: #fff;
  box-shadow: 0px 0px 50px 0px rgba(0, 0, 0, 0.10);
  // border: 2px solid var(--accent-color);
  color: #fff;
  padding: 30px 45px;
  border-radius: 20px;
}

.close {
  position: absolute;
  right: 20px;
  top: 25px;
  cursor: pointer;
}
</style>
