<script setup lang="ts">
import { Primitive } from 'radix-vue'
import { cn } from '@/lib/utils'

interface LabelProps {
  class?: any
  for?: string
}

defineProps<LabelProps>()
</script>

<template>
  <Primitive
    as="label"
    :class="cn('text-sm font-medium leading-none peer-disabled:cursor-not-allowed peer-disabled:opacity-70', $props.class)"
    :for="$props.for"
  >
    <slot />
  </Primitive>
</template>