<template>
  <RadioGroupRoot
    :class="cn('grid gap-2', $attrs.class as string)"
    v-bind="$attrs"
    :model-value="modelValue"
    @update:model-value="$emit('update:modelValue', $event)"
  >
    <slot />
  </RadioGroupRoot>
</template>

<script setup lang="ts">
import { RadioGroupRoot } from 'radix-vue'
import { cn } from '@/lib/utils'

defineProps<{
  modelValue?: string
}>()

defineEmits<{
  'update:modelValue': [value: string]
}>()
</script>
