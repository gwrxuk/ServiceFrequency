<script setup lang="ts">
import { cn } from '@/lib/utils'

interface Props {
  class?: any
}

defineProps<Props>()
</script>

<template>
  <div :class="cn('rounded-lg border bg-card text-card-foreground shadow-sm', $props.class)">
    <slot />
  </div>
</template>