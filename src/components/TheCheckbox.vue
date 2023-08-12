<template>
  <div class="checkbox">
    <div
      class="checkbox-wrapper"
      :class="{ 'checkbox-wrapper_checked': modelValue }"
      @click="onToggle"
    >
      <IconCheck class="checkbox-wrapper__icon" />
    </div>
    <label class="checkbox__label" :for="id">
      <span>
        <slot>{{ props.label }}</slot>
      </span>
      <input
        :id="id"
        ref="checkboxInputRef"
        class="checkbox__input"
        :name="name"
        type="checkbox"
        :value="modelValue"
        :disabled="disabled"
        @change="onChange"
      />
    </label>
  </div>
</template>
<script setup lang="ts">
import IconCheck from './icons/IconCheck.vue'
import { ref } from 'vue'

interface IProps {
  id: string
  name: string
  modelValue: boolean
  label?: string
  disabled?: boolean
}

const checkboxInputRef = ref<HTMLInputElement>()

const props = defineProps<IProps>()
const emit = defineEmits<{
  (event: 'update:modelValue', value: boolean): void
}>()

const onChange = (event: Event) => {
  const element = event.target as HTMLInputElement
  const isChecked = element.checked

  emit('update:modelValue', isChecked)
  console.log(isChecked)
}

const onToggle = () => checkboxInputRef.value?.click()
</script>
<style lang="scss">
.checkbox {
  display: flex;
  align-items: center;
  gap: 16px;
  cursor: pointer;

  &__input {
    display: none;
  }

  &__label {
    cursor: pointer;
  }
}

.checkbox-wrapper {
  width: 24px;
  height: 24px;
  border: 1px solid black;
  border-radius: 5px;

  &:hover {
    border: 1px solid black;
  }

  &_checked {
    .checkbox-wrapper__icon {
      opacity: 1;
    }
  }

  &__icon {
    opacity: 0;
    stroke: black;
    width: 24px;
    height: 24px;
  }
}
</style>
